# Rate

You can limit the amount of request by `time/rate` package. Here is a simple example:

```go
func main() {
	// Create a limiter with a rate limit of 1 event per second
	limiter := rate.NewLimiter(1, 1)

	// Simulate a burst of 5 events
	for i := 0; i < 10; i++ {
		// Wait for the limiter to allow an event
		if err := limiter.Wait(context.TODO()); err != nil {
			fmt.Printf("Error: %v\n", err)
			return
		}
		// Perform some operation (e.g., handle request)
		fmt.Println("Event processed")
	}
}
```

Maybe it is better to show an error instead of waiting for the rate limiter:

```go
func main() {
	// Create a limiter with a rate limit of 1 event per second
	limiter := rate.NewLimiter(1, 1)

	// Simulate a burst of 5 events
	for i := 0; i < 10; i++ {
		// Reject the requests
		if !limiter.Allow() {
			fmt.Println("can not this process time:", i*400)
		} else {
			fmt.Println("Event processed")
		}
		time.Sleep(400 * time.Millisecond)
	}
}
```

## Rate Limit Middleware by IP Address

To set a limiter by IP address.

```go
package limiters

import (
	"golang.org/x/time/rate"
	"net/http"
	"sync"
)

var limiter *IPRateLimiter

type IPRateLimiter struct {
	ips map[string]*rate.Limiter
	mu  *sync.RWMutex
	r   rate.Limit
	b   int
}

func NewIPRateLimiter(r rate.Limit, b int) *IPRateLimiter {
	i := &IPRateLimiter{
		ips: make(map[string]*rate.Limiter),
		mu:  &sync.RWMutex{},
		r:   r,
		b:   b,
	}

	return i
}

func (i *IPRateLimiter) AddIP(ip string) *rate.Limiter {
	i.mu.Lock()
	defer i.mu.Unlock()

	lmtr := rate.NewLimiter(i.r, i.b)

	i.ips[ip] = lmtr

	return lmtr
}

func (i *IPRateLimiter) GetLimiter(ip string) *rate.Limiter {
	i.mu.Lock()
	lmtr, exists := i.ips[ip]

	if !exists {
		i.mu.Unlock()
		return i.AddIP(ip)
	}

	i.mu.Unlock()

	return lmtr
}

func ByIp(next http.Handler, refillRate rate.Limit, tokenBucketSize int) http.Handler {
	limiter = NewIPRateLimiter(refillRate, tokenBucketSize)
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		lmtr := limiter.GetLimiter(r.RemoteAddr)
		if !lmtr.Allow() {
			w.WriteHeader(http.StatusTooManyRequests)
			w.Header().Set("Content-Type", "application/json")
			w.Write([]byte(`{"error": "too many requests"}`))
			return
		}

		next.ServeHTTP(w, r)
	})
}
```

## By Key

Here we have bunch of keys in database. We should se the header and if the key is present in the database then we should apply the limitation, otherwise it limits using `By IP address`.

```go
package limiters

import (
	_ "database/sql"
	"golang.org/x/time/rate"
	"net/http"
	"snapp/db"
	"sync"
)

var klimiter *KeyRateLimiter

type KeyRateLimiter struct {
	keys map[string]*rate.Limiter
	mu   *sync.RWMutex
	r    rate.Limit
	b    int
}

func NewKeyRateLimiter(r rate.Limit, b int) *KeyRateLimiter {
	i := &KeyRateLimiter{
		keys: make(map[string]*rate.Limiter),
		mu:   &sync.RWMutex{},
		r:    r,
		b:    b,
	}

	return i
}

func (i *KeyRateLimiter) AddKey(key string) *rate.Limiter {
	i.mu.Lock()
	defer i.mu.Unlock()

	lmtr := rate.NewLimiter(i.r, i.b)

	i.keys[key] = lmtr

	return lmtr
}

func (i *KeyRateLimiter) GetKeyLimiter(key string) *rate.Limiter {
	i.mu.Lock()
	lmtr, exists := i.keys[key]

	if !exists {
		i.mu.Unlock()
		return i.AddKey(key)
	}

	i.mu.Unlock()

	return lmtr
}

func ByAppKey(next http.Handler, refillRate rate.Limit, tokenBucketSize int) http.Handler {
	klimiter = NewKeyRateLimiter(refillRate, tokenBucketSize)
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		lmtr := klimiter.GetKeyLimiter(r.Header.Get("X-App-Key"))

		con := db.GetConnection()
		rows, _ := con.Query("select * from app_keys")
		isExist := false
		var id int
		var key string
		for rows.Next() {
			_ = rows.Scan(&id, &key)
			if key == r.Header.Get("X-App-Key") {
				isExist = true
			}
		}

		if isExist {
			if !lmtr.Allow() {
				w.WriteHeader(http.StatusTooManyRequests)
				w.Header().Set("Content-Type", "application/json")
				w.Write([]byte(`{"error": "too many requests"}`))
				return
			}

			next.ServeHTTP(w, r)
		} else {
			ByIp(next, refillRate, tokenBucketSize).ServeHTTP(w, r)
		}
	})
}
```