# gols
Upload Garmin .FIT data to Garmin Connect

## Installation

1. Clone repo:
```
git clone "https://github.com/briancxx/gols.git"
```

2. Edit JSON data.  
  - Replace "username" and "password" values with credentials for Garmin Connect.
  - Add directories to check when script executed by adding each absolute path as a string to the "directories" array.
  
## Manual

```
python gols/gols.sh
```

## Automatic on Mount
