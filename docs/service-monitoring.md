## Monitoring

### Binaries:
- scollector [Bosun.org scollector](https://bosun.org/scollector "scollector mainpage")
    - `/opt/scollector/scollector -> /opt/scollector/scollector-linux-amd64`

### Configs:
- scollector.toml `/etc/scollector/scollector.toml`

### Init:
- scollector 
    - `/etc/rc.d/init.d/scollector`

### Scollector Configuration

```toml
Host = "home.xalg.im:8070"
Hostname = "wklive"
[tags]
  hostgroup="WestKingdom"
```

### Management

- Start scollector
  `service scollector start`
- Stop Scollector
 `service scollector stop`

### Logging
scollector sends errors and log into to /var/log/messages unless told specifically to do other.
