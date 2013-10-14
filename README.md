EC2Setup
========

Documenting/sharing how I get an EC2 instance up and running.

- - - 

This whole process takes about 20 mins including creating an AWS account.
(I recommend creating a new email address and brand new amazon account to take
full advantage of AWS "free tier" so you don't pay while you play!)

### Register for an Amason Webservices Account

If you don't already have an Amazon Web Services (AWS) account 
you will need to register for one:
http://aws.amazon.com and click the **Sign Up** button.

**Note**: Signup will ask you for **credit card** details,
don't worry you won't be charged anything if you stay within 
the [http://aws.amazon.com/free/](http://aws.amazon.com/free/) 
(this includes a **Free** *Micro* Instance)

Once you have registered for AWS, login via: http://aws.amazon.com/console/
You will see all the AWS services available to you.

![AWS Console Home](https://raw.github.com/nelsonic/EC2Setup/master/screenshots/AWS-console-home.png "AWS Console Home")

Click on **EC2** (Elastic Cloud Compute)


### Add your SSH Keys to AWS

To access your EC2 instance via the command line you will need to let AWS know 
what key you want to use to access it.

![AWS Add Keypair](https://raw.github.com/nelsonic/EC2Setup/master/screenshots/AWS-import-keypair-nelsonic.png "AWS Add Key Pair")


If you don't have any knowledge of SSH or public/private keys, don't worry,
its pretty simple. 

[Insert simple explanation of Public Key Crypto :-]


### CSecurity Group Definitions

In the right column/menu click on **Security Groups** 
and confirm your **default** security profile allows
inbound traffic on TCP **Port 22** (SSH) and **80** (http)
(by default these ports aren't open, you need to add them)

![AWS Add TCP Port Rules](https://raw.github.com/nelsonic/EC2Setup/master/screenshots/AWS-security-group-add-TCP-rule.png "AWS  Add TCP Port Rules")

I only added ports 22 and 80 for this simple setup,
![AWS TCP Port Rules Defined](https://raw.github.com/nelsonic/EC2Setup/master/screenshots/AWS-security-groups-rules-defined.png "AWS TCP Port Rules")

but to run other node.js processes on your server
e.g. for MongoExpress or RedisAdmin you need to open
these here in the Security Group/Profile.

### Create EC2 Instance

Setting up an ec2 can be daunting the first time.
Don't worry, the defaults are sensible and if you get stuck
there is always help available! :-)

e.g: http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html


#### Launch Instance Wizard

Click **Instances** in the right column/menu and then click **Launch Instance**:
![AWS EC2 Launch](https://raw.github.com/nelsonic/EC2Setup/master/screenshots/AWS-create-ec2-instance-step0-launch.png "AWS ec2 launch")


#### Pick an Amazon Machine Image

Select an Amamzon Machine Image (AMI) from the list of available AMIs:

![AWS Chose an AMI](https://raw.github.com/nelsonic/EC2Setup/master/screenshots/AWS-create-ec2-instance-step1-choose-ami.png "AWS Pick an AMI")

I recommend chosing a *bare-bones* Ubuntu Image for two reasons:
- You don't get any *LAMP* Cruft bundled in
- Ubuntu is simplest Linux distro and has the most (answered) FAQ questions if you get stuck!


#### Select Instance Type

Keep the default **Micro** instance.

![AWS Micro](https://raw.github.com/nelsonic/EC2Setup/master/screenshots/AWS-create-ec2-instance-step2-chose-instance-type.png "AWS Micro")

#### Select the Default Security Group

Select the Default Security Group (the one we edited above):

![AWS Security Group](https://raw.github.com/nelsonic/EC2Setup/master/screenshots/AWS-create-ec2-instance-step6-configure-security-group.png "AWS Security Group")


#### Select the Key

Select the Key you imported earlier (so you can access the instance from SSH)

![AWS Security Group](https://raw.github.com/nelsonic/EC2Setup/master/screenshots/AWS-create-ec2-instance-step4-select-existing-key-pair.png "AWS Security Group")


#### Review and Launch

Go over everything one last time and click **Launch**
![AWS Review and Launch](https://raw.github.com/nelsonic/EC2Setup/master/screenshots/AWS-create-ec2-instance-step7-review-and-launch.png "AWS Review and Launch")


#### Confirm the Insance is Running


![AWS instance running](https://raw.github.com/nelsonic/EC2Setup/master/screenshots/AWS-create-ec2-instance-step8-instance-running.png "AWS instance running")

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
![AWS install node.js ubuntu](https://raw.github.com/nelsonic/EC2Setup/master/screenshots/AWS-sudo-apt-get-install-nodejs.png "AWS install node.js on ubuntu")


type **y** in the terminal and [enter] to install.

Confirm what version of nodejs you have installed:

```terminal
node --version
```

```terminal
npm --version
```

**Note**: If you get an *error*:

**-bash: /usr/sbin/node: No such file or directory**

You will need to add the path, simply run this command in terminal
(while logged into the ec2 instance):

```terminal
export PATH=$PATH:/usr/bin
```

### Create Simple Node.js HTTP Server 

```terminal
vi app.js
```
paste in a simple http app:

```javascript
var http = require('http'),
	port = 80;

var server = http.createServer(function (request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.end("Node.js HTTP Server Running on Amazon EC2!\n");
}).listen(port);

console.log("Node HTTP Server running on "+port);
```

Start the app:

```terminal
node app
```

Now visit the IP address corresponding to your instance:

mine was: http://54.229.220.192

![node server running](https://raw.github.com/nelsonic/EC2Setup/master/screenshots/AWS-node-running.png "Node HTTP Server Working")

**Note**: your IP address will be different
you get it by changing the hyphens in numbers part of the Public DNS
to periods and then pasting that IP (V4) into your browser.


### Random Tips

If you get the error: **Write failed: Broken pipe**
you need to keep the connection to the server alive.
To resolve this, open your ssh config file:

```terminal
vi ~/.ssh/config
```

and add the follwing lines:

```
ServerAliveInterval 120
TCPKeepAlive yes
```

### Notes

- Install Node.js on Ubuntu: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager#ubuntu-mint
- SSH Keep Alive: http://stackoverflow.com/questions/13228425/write-failed-broken-pipe
- More detail: http://unix.stackexchange.com/questions/34004/how-does-tcp-keepalive-work-in-ssh
- Simple Node Server: http://howtonode.org/hello-node
- EC2 Guide: http://aws.amazon.com/documentation/ec2/





