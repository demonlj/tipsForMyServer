# WireGuard auto-reconnect function with dynamic server IP

## Step 1: Install WireGuard in app store
## Step 2: Enable on-demand function in WireGuard
## Step 3: Enable cron service in macOS
## Step 4: checkWireGuard.sh
```
#!/usr/local/bin/bash
#Author: SHI
#Created Time: 2020/05/22
#Script Description: IP reachable shell

ip="192.168.6.1"

echo "时间是：`date '+%Y-%m-%d %H:%M:%S'`" >> ~/Sync/checkWireGuard.sh.log

ping -c1 -W1 $ip >/dev/null

if [ $? -eq 0 ]; then
        echo -e "Ping result: $?\nWireGuard server $ip is up." >> ~/Sync/checkWireGuard.sh.log
else
        echo -e "Ping result: $?\nWireGuard server $ip is down.\nReconnecting..." >> ~/Sync/checkWireGuard.sh.log
	ps -ef |grep WireGuardNetworkExtension |awk '{print $2}'|xargs kill -9
fi
```

## ~~Step 5: crontab -e~~  For unkown reason, ping result in cron is wrong

```
*/6 * * * * bash /Users/jason/Sync/checkWireGuard.sh >> //Users/jason/Sync/checkWireGuard.sh.log
```

## Step 5: launchagent

```
touch ~/Desktop/stdout
vim ~/Library/LaunchAgents/com.jason.checkWireGuard.plist
```

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <!-- Label唯一的标识 -->
    <key>Label</key>
    <string>com.jason.checkWireGuard</string>
    <!-- 指定要运行的脚本 -->
    <key>ProgramArguments</key>
    <array>
        <string>/Users/jason/Sync/checkWireGuard.sh</string>
    </array>
    <!-- 指定运行的时间 -->
    <key>StartCalendarInterval</key>
    <dict>
        <key>Minute</key>
        <integer>40</integer>
        <key>Hour</key>
        <integer>8</integer>
    </dict>
    <!-- 时间间隔 -->
    <key>StartInterval</key>
    <integer>360</integer>
    <key>StandardOutPath</key>
    <!-- 标准输出文件 -->
    <string>/Users/jason/Desktop/stdout</string>
    <!-- 标准错误输出文件，错误日志 -->
    <key>StandardErrorPath</key>
    <string>/Users/jason/Desktop/error.txt</string>
</dict>
</plist>
```

```css
launchctl load -w com.jason.checkWireGuard.plist
```

```css
launchctl start -w com.jason.checkWireGuard.plist
```
