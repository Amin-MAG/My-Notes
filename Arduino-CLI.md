First create a configuration file

```bash
arduino-cli config init
```

You can add some boards in this configuration file

```bash
vim /home/amin/.arduino15/arduino-cli.yaml
```

Here is an example format of this YAML file.

```yml
board_manager:
  additional_urls:
    - https://raw.githubus
    - ercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
daemon:
  port: "50051"
directories:
  data: /home/amin/.arduino15
  downloads: /home/amin/.arduino15/staging
  user: /home/amin/Arduino
library:
  enable_unsafe_install: false
logging:
  file: ""
  format: text
  level: info
metrics:
  addr: :9090
  enabled: true
output:
  no_color: false
sketch:
  always_export_binaries: false
updater:
  enable_notification: true
```

## Sketch

```bash
# Create new sketch
arduino-cli sketch new honeypot
```

## Board

```bash
# connected boards
arduino-cli board list

# All installed boards
arduino-cli board listall
```

## Install a new board

```bash
# Add the url to the configuration file
arduino-cli core update-index

# Start downloading the packages
arduino-cli core install esp32:esp32
```

## Compile and Upload

```BASH
arduino-cli compile -fqbn esp32:esp32:wrover <SKETCH_NAME>
arduino-cli upload -p /dev/ttyUSB0 --fqbn esp32:esp32:esp32wrover <SKETCH_NAME>
```

## Monitor serial port

```bash
arduino-cli monitor -p /dev/ttyUSB0
```