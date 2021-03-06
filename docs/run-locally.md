---
id: run-locally
title: Run Locally
sidebar_label: Run Locally
---

<br/>
Note that this is an **advanced process**. Some of the concepts used in this section are not easy for the beginners. In order to run CapRover on your local machine (just for testing and development) you need Docker installed on your machine.


CapRover, by default, uses `http://captain.captain.localhost`. In most systems, `captain.captain.localhost` resolves to local ip address of the machine, i.e. 12.0.0.1 and therefore no additional work is needed.

> However, if it doesn't you need to point `*.captain.local` to `127.0.0.1` or `192.168.1.2` (your local ip). **NOTE** that `etc/hosts` won't be enough as Captain needs a wildcard entry and `etc/hosts` does not allow wildcards, i.e. `*.something`. On ubuntu 16, `dnsmasq` (a local DNS server) is built-in. So, it's as simple of editing this file: `/etc/NetworkManager/dnsmasq.d/dnsmasq-localhost.conf` (create if does not exist) And add this line to it: `address=/captain.local/192.168.1.2` where `192.168.1.2` is your local IP address. To make sure you have `dnsmasq`, you can run `which dnsmasq` on your terminal, if it's available, path of it will be printed on the terminal, otherwise, there won't be anything printed on your terminal.
Note: For Ubuntu 18, read https://askubuntu.com/questions/1029882/how-can-i-set-up-local-wildcard-127-0-0-1-domain-resolution-on-18-04


To verify you have both prerequisites mentioned above:
- Run `docker version` and make sure your version is at least the version mentioned in the [docs](get-started.md#c-install-docker-on-server-at-least-version-1706x) 
- Run `nslookup randomstring123.captain.local` and make sure it resolves to `127.0.0.1` or your local ip (something like `192.168.1.2`):
```
Server:		127.0.1.1
Address:	127.0.1.1#53

Name:	randomstring123.captain.local
Address: 192.168.1.2
```


Once you confirmed that you have the prereqs ready, you can go ahead and install Captain on your machine, similar to what you do on server. Make sure you run as a user with sufficient permission, i.e. `sudo` on linux based systems. Just follow the steps outlined here: [Captain Installation](get-started#step-1-captain-installation)

**EXCEPT** 
Do not run `caprover serversetup`. Instead, go to http://captain.captain.local:3000 and manually set root domain to `captain.local`. DO NOT enable/force HTTPS. Obviously, you cannot enable HTTPS on your local domain (captain.local).

Once you set your root domain as `captain.local`, use `caprover login` and enter `captain.captain.local` as your captain URL and `captain42` as your default password. 


**NON-LINUX USERS**
You need to add `/captain` to shared paths.
To do so, click on the Docker icon -> Setting -> File Sharing and add `/captain`



You are set!


## Troubleshooting:


As mentioned above, running a local machine is an advanced task and might fail due to different reasons, depending on error your solution is different. For example, if you get the following error:

```
Captain Starting ...
Installing Captain Service ...
December 18th 2017, 11:51:11.295 pm    Starting swarm at 34.232.18.13:2377
Installation failed.
{ Error: (HTTP code 400) bad parameter - must specify a listening address because the address to advertise is not recognized as a system address, and a system's IP address to use could not be uniquely identified
    at /usr/src/app/node_modules/docker-modem/lib/modem.js:254:17
    at process._tickCallback (internal/process/next_tick.js:180:9)
  reason: 'bad parameter',
  statusCode: 400,
  json:
   { message: 'must specify a listening address because the address to advertise is not recognized as a system address, and a system\'s IP address to use could not be uniquely identified' } }
```
You can try this:

```
docker run -e "MAIN_NODE_IP_ADDRESS=192.168.1.2" -v /var/run/docker.sock:/var/run/docker.sock caprover/caprover
```

and replace `192.168.1.2` with your own local IP.
