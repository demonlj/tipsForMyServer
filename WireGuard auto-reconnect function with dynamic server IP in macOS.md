# WireGuard auto-reconnect function with dynamic server IP

## Step 1: Install WireGuard in app store
## Step 2: Enable on-demand function in WireGuard
## Step 3: Enable cron service in macOS
## Step 4: checkWireGuard.sh
```
#!/usr/bin/bash
#Author: SHI
#Created Time: 2020/05/22
#Script Description: IP reachable shell

# server ip
ip="192.168.6.1"

echo "时间是：`date '+%Y-%m-%d %H:%M:%S'`"

ping -c6 $ip >/dev/null

if [ $? -eq 0 ]; then
        echo "WireGuard server $ip is up."
else
        echo "WireGuard server $ip is down.\rReconnecting..."
	ps -ef |grep WireGuardNetworkExtension |awk '{print $2}'|xargs kill -9
fi
```

## Step 5: crontab -e

```
*/6 * * * * bash /Users/jason/Sync/checkWireGuard.sh >> //Users/jason/Sync/checkWireGuard.sh.log
```
