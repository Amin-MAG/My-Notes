# Testify

## Mocking

This is how we define some mock structs

```go
import (
	"context"
	"github.com/stretchr/testify/mock"
	"gitlab.snapp.ir/Map/sdk/smapp-sdk-go/services/reverse"
)

type MockReverseClient struct {
	mock.Mock
}

func (m *MockReverseClient) GetFrequentWithContext(ctx context.Context, lat, lon float64, options reverse.CallOptions) (reverse.FrequentAddress, error) {
	args := m.Called(ctx, lat, lon, options)
	return args.Get(0).(reverse.FrequentAddress), args.Error(1)
}
```

And then we use it like 

```go
client := new(MockReverseClient)
client.On("GetFrequentWithContext", ctx, 29.6454, 52.4782, reverse.CallOptions{}).Return(
	reverse.FrequentAddress{
		Address:        TestAddress01["address"],
		EnglishAddress: TestAddress01["address_en"],
	}, nil,
).Times(1)
client.On("GetFrequentWithContext", mock.Anything, mock.Anything, mock.Anything, mock.Anything).Return(
	reverse.FrequentAddress{}, errors.New("timeout error for SDK connection"),
)
```