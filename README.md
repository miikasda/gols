# gols
Upload Garmin .FIT data to Garmin Connect

## Installation

### 1. Clone repo:
```
git clone "https://github.com/briancxx/gols.git"
```

### 2. Install dependencies:
```
pip install bs4
```

### 3. Edit JSON data:
  - Replace "username" and "password" values with credentials for Garmin Connect.
  - Add directories to check when script executed by adding each absolute path as a string to the "directories" array.
  
  
## Manual

```
python gols/gols.sh
```

## Automatic on Mount
Automatically running *gols.py* when watch is mounted requires using *fstab* and *systemd*.

### 1. Get UUID or LABEL data from watch:
```
sudo blkid
```

### 2. Create fstab entry to automatically mount watch to directory when connected:
```
sudo nano /etc/fstab
```

```
LABEL="GARMIN"  /media/garmin vfat  auto,nofail,rw,user,uid=1000  0 2
```
Use the UUID or LABEL that `sudo blkid` found.

### 3. Add systemd service to run Python script after mount:

```
sudo nano /etc/systemd/system/gols.service
```

```
[Unit]
Description=Run gols.py on watch mount
Requires=media-garmin.mount
After=media-garmin.mount

[Service]
ExecStart=/home/user/gols/gols.py

[Install]
WantedBy=media-garmin.mount
```

### 4. Start systemd service:
```
sudo systemctl daemon-reload
sudo systemctl start gols.service
sudo systemctl enable gols.service
```

Note: Make sure to update the header in *gols.py* to match the correct location of Python
