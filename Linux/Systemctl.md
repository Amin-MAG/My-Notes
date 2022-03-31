# Systemctl

`Systemd` stores configuration for services in two places. The first is `/lib/systemd/system/`, where you’ll find configuration for many services on your system. Most software installs install services here. The second is `/etc/systemd/system/`, which overrides the `/lib/systemd` directory and is generally used to place user-created services in. There’s also `/etc/systemd/users/`, which runs services for individual logged-in users, such as fetching mail.

## Service

Many of these services don’t run all the time like nginx or MySQL would. You can print out a list of services currently in use swith:

```bash
service --status-all
```

Services with a “+” symbol are running, and services with a “-” symbol are currently stopped. You can view more detailed information with:

```bash
service status nginx
```

## `Journalctl`

Since services run in the background, they don’t log their output to your console, instead logging output to the systemd journal. The “status” command will show the last few lines of this journal, but you can read it directly with:

```
journalctl -fn 50 -u nginx
```

This command prints the most recent 50 log entries (`-n`) from the nginx service (`-u`). It is configured to print everything and start at the bottom, following new log entries as they are created (`-f`).

## Create basic service

At the moment, you probably just want to configure your application as a basic service. To do this, you’ll have to create a new unit file, which you’ll want to place in `/etc/systemd/system/` and name with a `.service` extension:

```bash
touch /etc/systemd/system/myapp.service
```

Unit files have a few different sections, but overall will look something like this:

```
[Unit]
Description=Example Service
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=1
User=serviceuser
ExecStartPre=
ExecStart=/path/to/executable [options]
ExecStartPost=
ExecStop=
ExecReload=

[Install]
WantedBy=multi-user.target
```

### Unit

First up, the [Unit] section, which defines a bunch of metadata about the unit. The After= directive can be used to delay unit activation until another unit is started, such as network, or another service, like `mysql.service` This doesn’t make it a hard dependant of that service, though you can do that with the Requires= or Wants= directives. This section also configures the maximum number of times the unit will attempt to be started before `systemd` gives up entirely; since you probably want it to keep trying, you can set this to 0 to disable this behavior.

### Service

Next is the `[Service]` section, specific to service unit files. This is where you’ll configure the Exec options. `User` will run the service as a certain user. You can set this to your personal user account, root, or a custom-made service account. Just make sure the user has enough permissions to do its job.

There are a few different directives here for specifying programs to run. `ExecStartPre` will run first, allowing you to do any setup necessary before the service really starts. `ExecStart` is the main executable. `ExecStartPost` runs afterward, and `ExecStop` is run when the service shuts down. `ExecReload` is a special directive and is used when you call “reload” instead of restart. This allows you to perform run-time reloading of configuration, provided your application has the capability.

### Install

Lastly, the [Install] section, which defines some more behaviour related to how `systemd` handles the unit. This is most commonly used to specify the `WantedBy=` directive, which is used to tell `systemd` when to start your service and creates symlinks between targets and their dependant units. If you’re unsure which target to use, `multi-user.target` will run services at startup after almost everything is loaded.

### Apply Changes

systemctl daemon to update with your changes:

```bash
sudo systemctl daemon-reload
```

And enable it (which will run it at boot, per the unit config):

```bash
sudo systemctl enable myapp
```

And then start the service:

```bash
sudo systemctl start myapp
# OR 
sudo service myapp start
```

## Remove a service

To remove a service

```bash
systemctl stop [servicename]
systemctl disable [servicename]
rm /etc/systemd/system/[servicename]
rm /etc/systemd/system/[servicename] # and symlinks that might be related
rm /usr/lib/systemd/system/[servicename] 
rm /usr/lib/systemd/system/[servicename] # and symlinks that might be related
systemctl daemon-reload
systemctl reset-failed
```

## Resources

Most of the article is from this link bellow.

[How To Add Your Own Services to systemd For Easier Management](https://www.cloudsavvyit.com/3092/how-to-add-your-own-services-to-systemd-for-easier-management/)

[Creating a Linux service with systemd](https://medium.com/@benmorel/creating-a-linux-service-with-systemd-611b5c8b91d6)