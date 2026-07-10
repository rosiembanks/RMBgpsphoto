# RMBphotogps
Bash script to open Google Maps to coordinates in a photo's meta-data

I am sure there are many out there, but I couldn't find one.  So I cobbled this one together.  You could probably reduce it to fewer actual lines.

**Dependencies**

- exiftool
- xdg-open

**Bash script**
```
#!/bin/bash
# RMBgpsphoto.sh
# Open GoogleMaps to GSP coordinates per photo meta data
# Usage
#        RMBgpsphoto.sh filename.ext
# e.g.   RMBgpsphoto.sh myphoto.jpg

# Dependencies
#    exiftool
#    xdg-open

# Echo filename
echo "Filename: "$1

# Pull GPS coordinates from image metadata & display
gpsvalue="$(exiftool -c '%.6f' -GPSPosition $1)"
echo $gpsvalue

#GPS coordinates format will be like this:
#GPS Position                    : 35.882294 N, 78.676522 W
#123456789+123456789+123456789+123456789+

# Need to construct URL like this:
# ........................GPS Position                    : 35.882294 N, 78.676522 W
# target URL will be like https://www.google.com/maps/place/35.882294 N, 78.676522 W

# Remove leading text.  Probably a better way to do this.
newgpsvalue=${gpsvalue:34}

# Construct xdg-open command, including construction of the full URL
newurl="xdg-open \"https://www.google.com/maps/place/"$newgpsvalue"\""
echo "Command: "$newurl

# Execute contents of variable newurl as a command
echo "$newurl" | bash
```

