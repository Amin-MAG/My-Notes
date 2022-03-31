# Golang Mock

Here is the `foo.go` file:

```go
type Foo interface {
	Do(age int) int
}
```

So to generate the mock file using the `mockgen` we use:

```bash
mockgen -package=mock -source=model/foo.go > mock/foo_mock.go
```

The generated file looks like this:

```go
// MockFoo is a mock of Foo interface.
type MockFoo struct {
	ctrl     *gomock.Controller
	recorder *MockFooMockRecorder
}

// MockFooMockRecorder is the mock recorder for MockFoo.
type MockFooMockRecorder struct {
	mock *MockFoo
}

// NewMockFoo creates a new mock instance.
func NewMockFoo(ctrl *gomock.Controller) *MockFoo {
	mock := &MockFoo{ctrl: ctrl}
	mock.recorder = &MockFooMockRecorder{mock}
	return mock
}

// EXPECT returns an object that allows the caller to indicate expected use.
func (m *MockFoo) EXPECT() *MockFooMockRecorder {
	return m.recorder
}

// Do mocks base method.
func (m *MockFoo) Do(age int) int {
	m.ctrl.T.Helper()
	ret := m.ctrl.Call(m, "Do", age)
	ret0, _ := ret[0].(int)
	return ret0
}

// Do indicates an expected call of Do.
func (mr *MockFooMockRecorder) Do(age interface{}) *gomock.Call {
	mr.mock.ctrl.T.Helper()
	return mr.mock.ctrl.RecordCallWithMethodType(mr.mock, "Do", reflect.TypeOf((*MockFoo)(nil).Do), age)
}
```

Let's create a `foo_test.go` class for testing:

```go
func TestFoo(t *testing.T) {
	ctrl := gomock.NewController(t)
	defer ctrl.Finish()

	// Create the mock foo
	m := mock.NewMockFoo(ctrl)

	m.EXPECT().Do(2983).DoAndReturn(func(_ int) int {
		time.Sleep(500 * time.Millisecond)
		return 32
	}).AnyTimes()

	m.EXPECT().Do(42).DoAndReturn(func(_ int) int {
		time.Sleep(1200 * time.Millisecond)
		return 929
	})

	m.EXPECT().Do(gomock.Not(gomock.Eq(42))).Return(412).AnyTimes()

	fmt.Println(m.Do(2983))
	fmt.Println(m.Do(2983))
	fmt.Println(m.Do(42))
	fmt.Println(m.Do(21))
}
```

# Repository

[GitHub - golang/mock: GoMock is a mocking framework for the Go programming language.](https://github.com/golang/mock)

# References

[A GoMock Quick Start Guide](https://betterprogramming.pub/a-gomock-quick-start-guide-71bee4b3a6f1)