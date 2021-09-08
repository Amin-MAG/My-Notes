# Go Cron

Time scheduling for Go language:

```go
var log = logrus.New()

var (
	fiveInterval  = 0
	threeInterval = 0
	sevenInterval = 0
)

func main() {
	log.Infoln("Hello World!")

	s := gocron.NewScheduler(time.UTC)
	_, _ = s.Every(500).Millisecond().Do(func() {
		fiveInterval++
		log.Infof("Run Interval 5 for %d time", fiveInterval)
	})

	_, _ = s.Every(300).Millisecond().Do(func() {
		threeInterval++
		log.Infof("Run Interval 3 for %d time", threeInterval)
	})

	_, _ = s.Every(700).Millisecond().Do(func() {
		sevenInterval++
		log.Infof("Run Interval 7 for %d time", sevenInterval)
	})

	s.StartAsync()
	s.StartBlocking()
}
```