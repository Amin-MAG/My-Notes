# Fluentd

Fluentd is something like Logstash. It receives logs from multiple sources, processes them, and in the end, it sends them to another service. It is possible to be another Fluentd, Elastic, etc.

## It's Safe

This system is safe because it stores the logs locally until it makes sure they have been sent to the output destination.

Besides that, It uses a retry mechanism to send the logs to the destination. If the output (like elastic) goes down, You don't need to worry about the records. They have been stored on the Fluentd local disk, and It will send them again.

## Plugins 

The Fluentd plugins are the most potent components of this system. They're some extendable components defining inputs, outputs, filters, buffering, parsing, formatting, and so on.

### Input and Output Plugins

Fluentd can receive and tag the logs from different sources.
These sources are defined with input plugins. Input plugins have serveral types. For example, It can receive logs through a TCP port, HTTP protocol, files, or other types. On the other hand, there are also some output plugin types (e.g., elastic). You can choose a tag and use a plugin to send it to one or more outputs.

### Filter Plugins

You can mutate the receiving logs and process them. For example, you can add the node name.

### Parser and Formatter Plugins

The Parser plugins are another Fluentd powerful plugins that can parse logs with different conventions and turn them into a standard log. Then the Formatter plugins specify the output style of logs.
