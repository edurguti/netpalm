# NetPalm

Why NetPalm?
Netpalm is a ReST API into your dusty old network devices, NetPalm makes it easy to push and pull network state from your web apps.
NetPalm leverages popular [napalm](https://github.com/napalm-automation/napalm) and [netmiko](https://github.com/ktbyers/netmiko) library's for network device communication, these powerful libs supprt a vast number of vendors and OS

## Netpalm Features

- Asynchronous parallel processing
- Task oriented
- Per device configuration queuing (Ensure you dont overload your VTY's)
- Standard ReST interface
- Large amount of supported multivendor devices ( cheers to the netmiko & napalm lads )
- Included postman collection of examples

## Using Netpalm

### Code
All of the code you need is located in this repo. Clone the repo and access it with the following commands:
```
git clone https://github.com/tbotnz/netpalm
cd netpalm
```

#### Concepts
Netpalm acts as a ReST broker for NAPALM and Netmiko.
You make an API call to netpalm and it will establish a queue to your device and start sending configuration
![np](/images/netpalm_concept.png)Format: !

#### Get config from a network device
Post some config to the getconfig route

### Postman example
getconfig method
![np](/images/netpalm_eg_1.png)Format:!

check response
![np](/images/netpalm_eg_2.png)Format:!

#### Get config from a network device using the librarys arg
netpalm also supports all arguments for the transport libs, simply pass them in as below
![np](/images/netpalm_eg_3.png)Format:!

check response
![np](/images/netpalm_eg_4.png)Format:!

##### netmiko
```
curl --location --request POST '127.0.0.1:9000/getconfig' \
--header 'x-api-key: 2a84465a-cf38-46b2-9d86-b84Q7d57f288' \
--header 'Content-Type: application/json' \
--data-raw '{
    "host":"10.0.2.33",
    "library": "netmiko",
    "driver":"cisco_ios",
    "command": "show run | i hostname",
    "username": "admin",
    "password": "admin"
}      '
```

##### napalm
```
curl --location --request POST '127.0.0.1:9000/getconfig' \
--header 'x-api-key: 2a84465a-cf38-46b2-9d86-b84Q7d57f288' \
--header 'Content-Type: application/json' \
--data-raw '{
    "host":"10.0.2.33",
    "library": "napalm",
    "driver":"ios",
    "command": "show run | i hostname",
    "username": "admin",
    "password": "admin"
}      '
```

#### Set Config on a network device

##### netmiko
```
curl --location --request POST '127.0.0.1:9000/setconfig' \
--header 'x-api-key: 2a84465a-cf38-46b2-9d86-b84Q7d57f288' \
--header 'Content-Type: application/json' \
--data-raw '{
    "host":"10.0.2.33",
    "library": "netmiko",
    "driver":"cisco_ios",
    "command": "hostname cat",
    "username": "admin",
    "password": "admin"
}'
```

##### napalm
```
curl --location --request POST '127.0.0.1:9000/setconfig' \
--header 'Content-Type: application/json' \
--header 'x-api-key: 2a84465a-cf38-46b2-9d86-b84Q7d57f288' \
--data-raw '{
    "host":"10.0.2.33",
    "library": "napalm",
    "driver":"ios",
    "config": ["hostname testing123", "int loopback01\n ip address 192.168.1.3 255.255.255.0\n"],
    "username": "admin",
    "password": "admin"
}      '
```

### Configuring Netpalm
edit the config.json file too set params as required
```
{
    "apikey": "2a84465a-cf38-46b2-9d86-b84Q7d57f288",
    "listen_port": 9000,
    "listen_ip":"0.0.0.0",
    "redis_task_ttl":500,
    "redis_task_timeout":500,
    "redis_server":"redis",
    "redis_port":6379,
    "redis_core_q":"process"
}
```

### Container Installation
ensure you first have docker installed
```
sudo apt-get install docker.io
sudo apt-get install docker-compose
```

clone in netpalm
```
git clone https://github.com/tbotnz/netpalm.git
cd netpalm
```

build container
```
sudo docker build -t netpalm .
sudo docker-compose up -d
```

import the postman collection, set the ip addresses to that of your docker host and off you go (default netpalm port is 9000)
```
http://$(yourdockerhost):9000
```

## Netpalm slack channel
#netpalm on networktocode.slack.com

## License
All code belongs to that of its respective owners
