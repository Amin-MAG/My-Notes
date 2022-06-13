# Telnetlib

```python
import telnetlib

HOST = "localhost"
PORT = "1337"
tn = telnetlib.Telnet(HOST, PORT)

tn.read_until(b"login: ")
tn.write(user.encode('ascii') + b"\n")
```