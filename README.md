# social auth mock

`somok` is created to load test Auth0 social connection by mocking various social providers (intially facebook only).
It's a basic OAuth2 authorization server built on top of oauth2rize framework and uses loki.js as the inmemory database to store users and OAuth2 transaction state.

## AWS
### Ireland
[![](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=somok-cluster&templateURL=https://s3.us-east-2.amazonaws.com/nacho-dev/somok-aws-template.json)

### Ohio (us-east-2)
[![](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=somok-cluster&templateURL=https://s3.us-east-2.amazonaws.com/nacho-dev/somok-aws-template.json)

**Note:** Cloudformation Template works for: `us-east-1, us-east-2, us-west-1, us-west-2, eu-west-1, eu-central-1`


## Setup instructions

### Mock server

`git clone https://github.com/zamd/somok.git`

`cd somok/setup`

`sudo ./setup.sh`

Make sure `nginx.conf` is copied to `/etc/nginx` folder and certs are copied to `/etc/nginx/ssl` folder

`cd ..`

`nave use stable`

`node .`

To see debugging output use following:

`DEBUG=somok node .`

The mock facebook authorization server should be running and fronted by nginx

### Appliance

1. On all appliance nodes add the root certificate in `trusted roots`, that means:
```
copy setup/ca-certs/fabrikam.inter.crt &
setup/ca-certs/FabrikamRootCA.crt to /usr/share/ca-certificates/
```

2. run the following command and mark the above certs as trusted:
```
sudo dpkg-reconfigure ca-certificates
```

3. Edit the `/etc/hosts` and map :
```
MOCK_IP   facebook.com
MOCK_IP   www.facebook.com
MOCK_IP   graph.facebook.com
````

### Load Impact Script
Set up subscription in `loadImpact.com`.
Copy `/loadimpact/loadscript.lua` to a loadimpact scenario to run the load test

### Jmeter

If your intention is to run the [social.jmx](https://github.com/auth0/appliance-load-testing/blob/master/jmeter/social.jmx) script or any other [Jmeter](http://jmeter.apache.org/) script you need to trust `setup/ca-certs/fabrikam.inter.crt`
and  `setup/ca-certs/FabrikamRootCA.crt` certs
in the Java keystore.

1. Identify the jvm that Jmeter uses. This is generally your JAVA_HOME environment varaible. On Mac Usually is at
```
/Library/Java/JavaVirtualMachines/JDK_VERSION.jdk/Contents/Home/jre/lib/security
```

2. Register `setup/ca-certs/fabrikam.inter.crt`:
```
keytool -import -alias certificate_mock1 -file fabrikam.inter.crt -keystore cacerts -storepass changeit [Return]
```

3. Register `setup/ca-certs/FabrikamRootCA.crt`:
```
keytool -import -alias certificate_mock2 -file FabrikamRootCA.crt -keystore cacerts -storepass changeit [Return]
```


4. Don't forget to point facebook endpoints to mock server:
```
MOCK_IP   facebook.com
MOCK_IP   www.facebook.com
MOCK_IP   graph.facebook.com
```


4. Done [Jmeter](http://jmeter.apache.org/) is ready to go!!
