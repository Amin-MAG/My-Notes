# PlatformIO

To create a new project

```bash
pio init --board=esp32dev
```

To list the connected the boards

```bash
pio device list
```

To upload the binary

```bash
pio run --target upload
```

To Monitor the logs of the devices

```bash
pio device monitor --port /dev/cu.usbserial-0001  --baud 115200
```

To search between different platforms and devices

```bash
pio platforms search
```