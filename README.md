EC2Setup
========

Documenting/sharing how I get an EC2 instance up and running.

- - - 

If you don't already have an AWS account you will need to register for one:
http://aws.amazon.com and click the **Sign Up** button.

**Note**: Signup will ask you for **credit card** details,
don't worry you won't be charged anything if you stay within 
the [http://aws.amazon.com/free/](http://aws.amazon.com/free/) 
(this includes a **Free** *Micro* Instance)

Once you have registered for AWS, login via: http://aws.amazon.com/console/

### Add your SSH Keys to AWS



### Confirm Security Group Definitions

### Create EC2 Instance




### Connecting to your instance via SSH (Terminal/Console)

If you only have *one* key pair on your computer you can log into your EC2 Instance
using a command:

```terminal
ssh ubuntu@ec2-54-229-220-192.eu-west-1.compute.amazonaws.com
```

Otherwise you will need to specify the key in your ssh connection request:

```terminal
ssh -i ~/.ssh/nelsonic.pem ubuntu@ec2-54-229-220-192.eu-west-1.compute.amazonaws.com
```


### Install Node.js

Mercifully you no longer need to compile node.js from source (the good old days ;-)
Now there is an apt package you can install with a single command:

```terminal
sudo apt-get install nodejs npm
```

Confirm what version of nodejs you have installed:

```terminal
node --version
```

**Note**: If you get an *error*:

**-bash: /usr/sbin/node: No such file or directory**

You will need to add the path, simply run this command in terminal
(while logged into the ec2 instance):

```terminal
export PATH=$PATH:/usr/bin
```

### Create Simple Node.js HTTP Server 


```javascript
var http = require('http'),
	port = 80;

var server = http.createServer(function (request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.end("Node.js HTTP Server Running on Amazon EC2!\n");
}).listen(port);

console.log("Node HTTP Server running on "+port);
```

http://54.229.220.192


**Note**: your IP address will be different


### Tips

If you get the error: **Write failed: Broken pipe**
you need to keep the connection to the server alive.
To resolve this, open your ssh config file:

```terminal
vi ~/.ssh/config
```

and add the follwing lines:

```
ServerAliveInterval 120
TCPKeepAlive no
```

### Notes

- https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager#ubuntu-mint
- http://stackoverflow.com/questions/13228425/write-failed-broken-pipe
- http://howtonode.org/hello-node





