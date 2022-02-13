# Golang, Real-Time Techs

Here I'm going to implement some kinds of ways that we can implement real-time data transferring.

## TCP Chat

There is a TCP chat implementation in `tcp_chat/` directory.

To run the server you should build the project first.

```bash
$ go build .
$ ./chat

2021/08/12 03:19:35 starting tcp server on 8888...
```

Then you can connect to this server using telnet.

```bash
$ telnet <IP_ADDRESS> <PORT>
$ telnet localhost 8888
Trying 127.0.0.1...           
Connected to localhost.       
Escape character is '^]'.
```

Here are the commands:

- `/nick` to change the user name.
- `/join` to join or create new rooms
- `/rooms` to see available rooms to join.
- `/msg` to send a message
- `/quit` closing the TCP connection

### Resources

[packagemain #20: Building a TCP Chat in Go](https://www.youtube.com/watch?v=Sphme0BqJiY)

## Gorilla Web-Socket

Create chat application using gorilla web socket package. It has some benefits over the built-in net package in Go.

### Resource

[websocket/examples at master · gorilla/websocket](https://github.com/gorilla/websocket/blob/master/examples)

## Centralized Player - Echo

The goal is to create parties so that we can join them and listen to the music that they're streaming.

- `join_party` joins a party.
- `get_parties` shows all of the parties.
- `quit_party` quits the party.