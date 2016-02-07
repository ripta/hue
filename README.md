```
$ cat payload/discovery.bin | socat -t 5 STDIO UDP-DATAGRAM:239.255.255.250:1900,broadcast | grep '^LOCATION:'
LOCATION: http://192.168.0.9:80/description.xml
LOCATION: http://192.168.0.9:80/description.xml
LOCATION: http://192.168.0.9:80/description.xml
LOCATION: http://192.168.0.9:80/description.xml
LOCATION: http://192.168.0.9:80/description.xml
LOCATION: http://192.168.0.9:80/description.xml

$ ./register.rb 192.168.0.9
Attempting to connect to Hue bridge at 192.168.0.9
Hue bridge found
Please press the button on the bridge in the next 30 seconds
Hue bridge paired
```
