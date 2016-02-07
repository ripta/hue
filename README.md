
```
$ cat payload/discovery.bin | socat -t 5 STDIO UDP-DATAGRAM:239.255.255.250:1900,broadcast | grep '^LOCATION:'
LOCATION: http://192.168.0.9:80/description.xml
LOCATION: http://192.168.0.9:80/description.xml
LOCATION: http://192.168.0.9:80/description.xml
LOCATION: http://192.168.0.9:80/description.xml
LOCATION: http://192.168.0.9:80/description.xml
LOCATION: http://192.168.0.9:80/description.xml
```

Authenticate once:

```
$ curl -H 'Accept: application/json' -X POST --data '{"devicetype":"cli","username":"huebee-red"}' http://192.168.0.2/api
[{"error":{"type":101,"address":"","description":"link button not pressed"}}]
```

Press pair button on hue, and reauthenticate:

```
$ curl -H 'Accept: application/json' -X POST --data '{"devicetype":"cli","username":"huebee-red"}' http://192.168.0.2/api
[{"success":{"username":"huebee-red"}}]
```

Retrieve lights:

```
$ curl -s http://192.168.0.2/api/huebee-red | jq '.lights'
{
  ...
}

# or

$ curl -s http://192.168.0.2/api/huebee-red/lights | jq .
```

Retrieve specific light:

```
$ curl -s http://192.168.0.2/api/huebee-red/lights/3 | jq .
{
  "state": {
    "on": true,
    "bri": 254,
    "hue": 33845,
    "sat": 16,
    "effect": "none",
    "xy": [
      0.3763,
      0.3741
    ],
    "ct": 245,
    "alert": "none",
    "colormode": "ct",
    "reachable": true
  },
  "type": "Extended color light",
  "name": "Flood Light",
  "modelid": "LCT007",
  "manufacturername": "Philips",
  "uniqueid": "00:17:88:01:00:3f:8c:28-0b",
  "swversion": "66014919"
}

$ curl -X PUT -H 'Accept: application/json' --data '{"on":true,"transitiontime":25,"bri":255,"sat":255,"hue":51725}' http://192.168.0.2/api/huebee-red/lights/3/state
$ curl -X PUT -H 'Accept: application/json' --data '{"effect":"colorloop"}' http://192.168.0.2/api/huebee-red/lights/3/state
$ curl -X PUT -H 'Accept: application/json' --data '{"effect":"none"}' http://192.168.0.2/api/huebee-red/lights/3/state
$ curl -X PUT -H 'Accept: application/json' --data '{"on":true,"xy":[0.4124,0.3941],"bri":255}' http://192.168.0.2/api/huebee-red/lights/3/state
$ curl -X PUT -H 'Accept: application/json' --data '{"on":true,"xy":[0.3763,0.3741],"bri":255}' http://192.168.0.2/api/huebee-red/lights/3/state
$ curl -X PUT -H 'Accept: application/json' --data '{"alert":"select"}' http://192.168.0.2/api/huebee-red/lights/3/state

# Color temperature ("ct") in mireds (reciprocal megakelvins) -- https://en.wikipedia.org/wiki/Mired
# 153 = 6500K, 200 = flash photography, 500 = 2000K

$ curl -X PUT -H 'Accept: application/json' --data '{"ct":200}' http://192.168.0.2/api/huebee-red/lights/3/state
```
