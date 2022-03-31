# Go Bot

# Adapters

Adapters are objects responsible for controlling the other components. They are also implementing the Digital Writer interface.

# Drivers

Drivers are some general attributes and functionalities to modify their states. For example, about LED Driver we can toggle, set brightness, turn off, or turn on.

To create a new Diver we need to pass a Digital Writer beside the pin number. All controllers are Digital Writers. For example, a Raspi adapter implements the digital writer interface.

# Robot

The robot struct is an entity that manages a set of devices and connections with a working function. The connections and devices are going to be started and stopped automatically with the robot. The working function is a simple `func()` type object that will be executed in another goroutine as soon as all the devices and connections have been initialized and started.

# Change the LED state using an array

We have 2 Led the left and the right. Here is the code that we change the state of these two over time.

```bash
package main

import (
	"gobot.io/x/gobot/platforms/raspi"
	"time"

	"gobot.io/x/gobot"
	"gobot.io/x/gobot/drivers/gpio"
)

func main() {
	r := raspi.NewAdaptor()
	ledR := gpio.NewLedDriver(r, "13")
	ledL := gpio.NewLedDriver(r, "11")

	i := 0
	a := []int{
		1, 0, -1, 0, -1, 0, -1, 0, 1, 0, 1, 0, -1, 0, 1, 0,
	}

	work := func() {
		gobot.Every(500*time.Millisecond, func() {
			shouldBe := a[i%len(a)]
			switch shouldBe {
			case 1:
				if !ledR.State() {
					ledR.Toggle()
				}
				if ledL.State() {
					ledL.Toggle()
				}
			case 0:
				if ledR.State() {
					ledR.Toggle()
				}
				if ledL.State() {
					ledL.Toggle()
				}
			case -1:
				if ledR.State() {
					ledR.Toggle()
				}
				if !ledL.State() {
					ledL.Toggle()
				}
			}
			i++
		})
	}

	robot := gobot.NewRobot("bot",
		[]gobot.Connection{r},
		[]gobot.Device{ledR, ledL},
		work,
	)

	robot.Start()
}
```