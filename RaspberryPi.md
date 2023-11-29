# RaspberryPi

## Interact with GPIO using terminal

```bash
# Prints the state of all GPIO pins
raspi-gpio get
# Prints the state of GPIO pin 17
raspi-gpio get 17

# Sets GPIO pin 17 as an output
raspi-gpio set 17 op
# Sets GPIO pin 17 to drive high
raspi-gpio set 17 dh
# Sets GPIO pin 17 to drive low
raspi-gpio set 17 dl
```