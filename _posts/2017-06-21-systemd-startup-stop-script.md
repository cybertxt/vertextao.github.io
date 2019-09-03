# Make startup/stop script in systemd system

## 1. The script

Prepare the script you want to run, name it, let's say `/path/to/foo`.

**Make sure the head line is `#!/bin/sh`**

Example content:

```shell
#!/bin/sh

start() {
  exec # whatever you want etc
}

stop() {
  exec # whatever you want etc
}

case $1 in
  start|stop) "$1" ;;
esac
```

## 2. The unit file

It is a `.service` file in `/etc/systemd/system`, take `foo.service` for example.

Example content:

```config
[Unit]
Description=Power-off gpu

[Service]
Type=oneshot
ExecStart=/path/to/foo start
ExecStop=/path/to/foo stop
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

## 3. Enable it

`systemctl enable foo.service`