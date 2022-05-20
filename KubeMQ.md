# KubeMQ

## Sender and Receiver

The core functionality of KubeMQ messaging is sending and receiving messages.

**Senders** (publishers) can send one or many messages (stream) to one or many destinations (Channels). Sending a message does not require the set up of any predefined destination.

**Receiver** (subscribers/listeners) can receive messages from one or more senders on the same channel or a wildcards channel. Before a Receiver can receive any messages, a Subscription function is needed to register his interest in receiving messages from a senders designation.

## Deploy using Docker-compose

```yaml
version: '3.7'
services:
  kubemq:
    image: kubemq/kubemq:latest
    container_name: kubemq
	ports:
  	- "8080:8080"
  	- "9090:9090"
  	- "50000:50000"
	environment:
	  - KUBEMQ_HOST=kubemq
	  - KUBEMQ_TOKEN="Your KubeMQ Token Here"
	networks:
	  - backend
	volumes:
	  - kubemq_vol:/store
networks:
  backend:
volumes:
  kubemq_vol:
```

## Publish/Subscribe

Publishing and event

```go
import (  
   "context"  
   "github.com/kubemq-io/kubemq-go"
   "log"
)  
  
func main() {  
   // Create a new context with cancel  
   ctx, cancel := context.WithCancel(context.Background())  
   defer cancel()  
  
   // Create kubeMQ client  
   client, err := kubemq.NewClient(ctx,  
      kubemq.WithAddress("188.121.110.10", 50000),  
      kubemq.WithClientId("hello-world-sender"),  
      kubemq.WithTransportType(kubemq.TransportTypeGRPC))  
   if err != nil {  
      log.Fatal(err)  
   }   defer client.Close()  
  
   // Create a channel  
   channelName := "testing_event_channel"  
   err = client.E().  
      SetId("some-id").  
      SetChannel(channelName).  
      SetMetadata("some-metadata").  
      SetBody([]byte("hello kubemq - sending single event")).  
      Send(ctx)  
   if err != nil {  
      log.Fatal(err)  
   }
}
```


Subscribing Events

```go
import (  
   "context"  
   "fmt"   
   "github.com/kubemq-io/kubemq-go"
   "log"
)  
  
func main() {  
   // Create a new context with cancel  
   ctx, cancel := context.WithCancel(context.Background())  
   defer cancel()  
  
   // Create the kubeMQ client  
   client, err := kubemq.NewClient(ctx,  
      kubemq.WithAddress("188.121.110.10", 50000),  
      kubemq.WithClientId("hello-world-subscriber"),  
      kubemq.WithTransportType(kubemq.TransportTypeGRPC))  
   if err != nil {  
      log.Fatal(err)  
   }   defer client.Close()  
  
   // Create a new channel  
   channelName := "testing_event_channel"  
   errCh := make(chan error)  
   
   // Subscribing
   eventsCh, err := client.SubscribeToEvents(ctx, channelName, "", errCh)  
   if err != nil {  
      log.Fatal(err)  
      return  
   }  
   for {  
      select {  
      case err := <-errCh:  
         log.Fatal(err)  
         return  
      case event, more := <-eventsCh:  
         if !more {  
            fmt.Println("Event Received, done")  
            return  
         }  
         log.Printf("Event Received:EventID: %s Channel: %s Metadata: %s Body: %s",  
            event.Id, event.Channel, event.Metadata, event.Body)  
      case <-ctx.Done():  
         return  
      }  
   }
}
```


## Basic CLI commands

```bash
# To install the CLI
curl -L https://github.com/kubemq-io/kubemqctl/releases/download/latest/kubemqctl_linux_amd64 -o /usr/local/bin/kubemqctl
chmod +x /usr/local/bin/kubemqctl

# 
```

