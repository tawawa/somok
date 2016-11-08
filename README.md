# social auth mock

`somok` is created to load test Auth0 social connection by mocking various social providers (intially facebook only). 
It's a basic OAuth2 authorization server built on top of oauth2rize framework and uses loki.js as the inmemory database to store users and OAuth2 transaction state.

## AWS
[![](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=somok-cluster&templateURL=https://raw.githubusercontent.com/zamd/somok/master/aws-cluster.json)

##Setup instructions

###Mock server

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

###Appliance
On all appliance nodes add the root certificate in `trusted roots`

copy `fabrikam.inter.crt` &  `FabrikamRootCA.crt` to `/usr/share/ca-certificates/`
run the following command and mark the above certs as trusted...

`sudo dpkg-reconfigure ca-certificates`

Edit the `/etc/hosts` and map `facebook.com/graph.facebook.com` to this mock authorization server. 

Set up subscription in `loadImpact.com`. 

Copy `/loadimpact/loadscript.lua` to a loadimpact scenario to run the load test
