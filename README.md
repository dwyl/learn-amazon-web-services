Tutorials for AWS services

* [EC2](#ec2setup)
* [S3 Static Website Hosting](#s3-static-website-hosting)

## EC2Setup
========

Documenting/sharing how I get an EC2 instance up and running.

- - -

This whole process takes about 20 mins including creating an AWS account.
(I recommend creating a new email address and brand new amazon account to take
full advantage of AWS "free tier" so you don't pay while you play!)

As always, if you have questions,
[raise an issue](https://github.com/dwyl/learn-amazon-web-services/issues/new)!
:sparkles:

### Register for an Amazon Webservices Account

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

Follow this tutorial: http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html



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

On Mac/Linux Open your **Terminal** application and execute
the follow SSH connection command:

(If you only have *one* key pair on your computer)

```terminal
ssh ubuntu@ec2-54-229-220-192.eu-west-1.compute.amazonaws.com
```

If you have more than one key pair on your computer
you will need to specify the key in your ssh connection request:

```terminal
ssh -i ~/.ssh/nelsonic.pem ubuntu@ec2-54-229-220-192.eu-west-1.compute.amazonaws.com
```

New to terminal? Try this guide: http://guides.macrumors.com/Terminal


### Install Node.js

Mercifully you no longer need to compile node.js from source (the good old days ;-)
Now there is a [great package](https://github.com/creationix/nvm) that allows
you to install any version of Node with just a few commands:

Install NVM:
```terminal
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```

Install the latest version of Node:
```terminal
nvm install node
```

Once that's installed update NPM:
```terminal
npm i -g npm
```

### Redirect Calls To Port 80

When installed with NVM Node doesn't have access to port 80 (ports under 1024
need root access). To fix this we'll need to redirect all traffic going to port
80 to 3000:

```terminal
sudo iptables -t nat -I PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 3000
```

### Create A Simple Node.js HTTP Server

```terminal
nano app.js
```

This will bring up `nano` which is a simple in terminal text editor!

paste in a simple http app:

```javascript
var http = require('http');
var port = 3000;

var server = http.createServer(function (request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.end("Node.js HTTP Server Running on Amazon EC2!\n");
}).listen(port);

console.log("Node HTTP Server running on "+port);
```

On linux this will be done with `shift + ctrl + v`, on mac it will be with `cmd + v`.

Once you've pasted this in you can exit by pressing `ctrl + x`, you will be
prompted to save the file.

Type `y` to say yes to saving your changes, and then nano will allow you to edit
the name of the file you write out to.
Leave the file name as is and press `enter`.

Then, start the app:

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
nano ~/.ssh/config
```

and add the following lines:

```
ServerAliveInterval 120
TCPKeepAlive yes
```


## S3 Static Website Hosting

An S3 bucket can be used to host a static website.

1. Start by creating a new S3 bucket in the AWS S3 console.

 ![s3 console](https://cloud.githubusercontent.com/assets/5912647/12922703/71dca7f4-cf4a-11e5-8cb2-80df7d8795ab.png)

 ![new bucket](https://cloud.githubusercontent.com/assets/5912647/12922707/78204cce-cf4a-11e5-8626-ad93fbd67ea7.png)


2. Select the 'Static Website Hosting' tab. Click 'Enable Website Hosting' and in the 	Index Document box add the name of the html file that will be your home page.

	![bucket created](https://cloud.githubusercontent.com/assets/5912647/12922719/7f829da0-cf4a-11e5-9aae-70be23b348d2.png)

	![static hosting](https://cloud.githubusercontent.com/assets/5912647/12922739/9780e33a-cf4a-11e5-9948-411b25500256.png)

	Click Save. Note down the url for your website. It will be of the form :

	`http://[bucket-name].s3-website-[region].amazonaws.com`

3. Select the permissions tab. We need to add a bucket policy that make the bucket content public so the website endpoint can show the website files.

	![permissions 1](https://cloud.githubusercontent.com/assets/5912647/12922752/a3de5aa4-cf4a-11e5-9a87-71e09dbd3206.png)

	Click 'Add Bucket Policy' and paste in the following policy:

	```js
	{
	  "Version":"2012-10-17",
	  "Statement":[{
		"Sid":"PublicReadForGetBucketObjects",
	        "Effect":"Allow",
		  "Principal": "*",
	      "Action":["s3:GetObject"],
	      "Resource":["arn:aws:s3:::example-bucket/*"
	      ]
	    }
	  ]
	}
	```

	Replace 'example-bucket' with the name of your bucket

	![permissions 2](https://cloud.githubusercontent.com/assets/5912647/12922762/b0c6c71a-cf4a-11e5-8fa5-f40f37c9cc83.png)

4. Upload the index document and other files for your website.

	![upload](https://cloud.githubusercontent.com/assets/5912647/12922773/bb8e1e46-cf4a-11e5-991d-3166631a8027.png)

	![upload 2](https://cloud.githubusercontent.com/assets/5912647/12922815/f1cbb8ec-cf4a-11e5-97b0-fda80b5940c2.png)

5. Test the website by entering the website URL in the browser!

	The uploading of files can be incorporated into your continuous integration process. **NB the names of the files need to be versioned to prevent the cached version being displayed after an update.**

### Notes

- Install Node.js on Ubuntu: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager#ubuntu-mint
- SSH Keep Alive: http://stackoverflow.com/questions/13228425/write-failed-broken-pipe
- More detail: http://unix.stackexchange.com/questions/34004/how-does-tcp-keepalive-work-in-ssh
- Simple Node Server: http://howtonode.org/hello-node
- EC2 Guide: http://aws.amazon.com/documentation/ec2/
- S3: Setting up a static website: http://docs.aws.amazon.com/AmazonS3/latest/dev/HostingWebsiteOnS3Setup.html
