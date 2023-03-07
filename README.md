# gols
Upload / Synchronize data from Garmin watch to Garmin Connect on linux.

## Why to use this fork?
- This fork supports uploading both activities and wellness (heartbeat, sleep, steps, etc.) data.
- Allows multiple synchronizations when using automatic upload (parent repository requires boot or systemctl reload after every auto sync).
- When using the fastSync option this fork is faster in upload than its parent.
  - fastSync works in most cases, only if some data is not synced you can force to upload all data from device.
  - Average synchronization time with fastSync is 7 sec vs 75 sec in parent repository.
- Easy to read program output and easy to verify source code.


## Installation

### 1. Clone repo:
```
git clone "https://github.com/miikasda/gols.git"
```

### 2. Install dependencies:
```
pip install beautifulsoup4 cloudscraper
```

### 3. Edit JSON data:
  - Replace "username" and "password" values with credentials for Garmin Connect.
  - Add directories to check when script executed by adding each absolute path as a string to the "directories" array.
  - Leave fastSync to true for faster upload, if you need to force reupload all data on device set it to false.

  Example gols.json used with Garmin Forerunner 35 to upload both activities and wellness data:

  ```
  {
  "username":"john@example.com",
  "password":"MyPassword",
  "fastSync":true,
  "directories":
    [
      "/media/john/GARMIN/GARMIN/ACTIVITY",
      "/media/john/GARMIN/GARMIN/MONITOR"
    ]
  }
  ```
  
## Manual Upload

```
python gols.py
```

## Automatic Upload on Mount
Automatically running *gols.py* when watch is mounted with *systemd*. 

### 1. Get watch mount unit:
```
systemctl list-units -t mount
```

### 2. Add systemd service to run Python script after mount:

```
sudo nano /etc/systemd/system/gols.service
```

```
[Unit]
Description=Run gols.py on watch mount
Requires=media-john-GARMIN.mount
After=media-john-GARMIN.mount

[Service]
ExecStart=/home/john/Documents/gols/gols.py
User=john

[Install]
WantedBy=media-john-GARMIN.mount

```
Use the mount unit found in Step 1 in following fields:
- `Requires`
- `After`
- `WantedBy`

Change the `User=john` to match your username returned by `whoami` and `ExecStart` point to path of *gols.py*.

### 3. Start systemd service:
```
sudo systemctl start gols.service
sudo systemctl enable gols.service
```

Note: Make sure to update the header in *gols.py* to match the correct location of Python and check that *gols.py* is executable:

```
chmod +x gols.py
```