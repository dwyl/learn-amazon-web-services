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

As always, if you have any questions, tweet me!
[@nelsonic](https://twitter.com/nelsonic)

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

![AWS Console](https://user-images.githubusercontent.com/22300773/28063290-97479866-6628-11e7-8a57-7d71c97419fa.png)

Click on **EC2** (Elastic Cloud Compute)


### Add your SSH Keys to AWS

To access your EC2 instance via the command line you will need to let AWS know
what key you want to use to access it.

![AWS Add Keypair](https://user-images.githubusercontent.com/22300773/28063289-9746d052-6628-11e7-8081-356b54e3e6ae.png)


If you don't have any knowledge of SSH or public/private keys, don't worry,
its pretty simple.

Follow this tutorial: http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html


### Security Group Definitions

In the left column/menu click on **Security Groups**
and confirm your **default** security profile allows
inbound traffic on TCP **Port 22** (SSH) and **80** (http)
(by default these ports aren't open, you need to add them)

![AWS Create Security Group](https://user-images.githubusercontent.com/22300773/28063555-5c709372-6629-11e7-9d50-de1207e70c21.png)

I only added ports 22 and 80 for this simple setup,
![AWS TCP Port Rules Defined](https://user-images.githubusercontent.com/22300773/28063554-5c707978-6629-11e7-99b1-78aaa7ef9088.png)

but to run other node.js processes on your server
e.g. for MongoExpress or RedisAdmin you need to open
these here in the Security Group/Profile.

### Create EC2 Instance

Setting up an ec2 can be daunting the first time.
Don't worry, the defaults are sensible and if you get stuck
there is always help available! :-)

e.g: http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html


#### Launch Instance Wizard

Click **Instances** in the left column/menu and then click **Launch Instance**:
![AWS EC2 Launch](https://user-images.githubusercontent.com/22300773/28063661-c833d2d6-6629-11e7-850c-28029f8ebcfe.png)


#### Pick an Amazon Machine Image

Select an Amamzon Machine Image (AMI) from the list of available AMIs:

![AWS Choose an AMI](https://user-images.githubusercontent.com/22300773/28063676-d8217d60-6629-11e7-9272-b96e83ab8232.png)

I recommend choosing a *bare-bones* Ubuntu Image for two reasons:
- You don't get any *LAMP* Cruft bundled in
- Ubuntu is a very beginner friendly Linux distro and has the most (answered) FAQ questions if you get stuck!


#### Select Instance Type

Keep the default **Micro** instance.

![AWS micro](https://user-images.githubusercontent.com/22300773/28063704-fbcb35f8-6629-11e7-9579-3cb01011a95e.png)

#### Select the Default Security Group

Select the Default Security Group (the one we edited above):

![AWS Security Group](https://user-images.githubusercontent.com/22300773/28063774-38b81f12-662a-11e7-940a-62cc7ee072eb.png)

#### Review and Launch

Go over everything one last time and click **Launch**
![AWS Review and Launch](https://user-images.githubusercontent.com/22300773/28063850-742e4134-662a-11e7-9bb4-303fdd6664e1.png)

#### Select the Key

After launch you will be prompted to select a key.

Select the Key you imported earlier (so you can access the instance from SSH):

![AWS Select Key](https://user-images.githubusercontent.com/22300773/28063873-8588371e-662a-11e7-89aa-5ce980e27cd8.png)

#### Confirm the Instance is Running


![AWS instance running](https://user-images.githubusercontent.com/22300773/28063940-c1baaff0-662a-11e7-99d0-1642c3511111.png)


### Connecting to your instance via SSH (Terminal/Console)

Make sure that your instance is selected and click connect. Copy the example
command from the dialogue, replacing `"<YOUR-KEY-NAME>.pem"` with the full path
to your key:

![AWS ssh dialogue](https://user-images.githubusercontent.com/22300773/28024954-fc8ed65c-6589-11e7-91dd-6abfed4da36b.png)

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
vi app.js
```

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

Start the app:

```terminal
node app
```

Now visit the IP address corresponding to your instance:

mine was: http://54.229.220.192

![node server running]

**Note**: your IP address will be different

You can get it by going to your AWS console page, going to your EC2 instances
and looking at the "IPv4 Public IP" section of the instance we just created.

![screenshot highlighting where to find Public IPv4 in AWS console ](https://user-images.githubusercontent.com/22300773/28064323-0b993c44-662c-11e7-8c8b-15297794e252.png)

### Random Tips

If you get the error: **Write failed: Broken pipe**
you need to keep the connection to the server alive.
To resolve this, open your ssh config file:

```terminal
vi ~/.ssh/config
```

and add the following lines:

```
ServerAliveInterval 120
TCPKeepAlive yes
```


## S3 Static Website Hosting

An S3 bucket can be used to host a static website.

1. Start by creating a new S3 bucket in the AWS S3 console.

 ![Create S3 bucket](https://user-images.githubusercontent.com/22300773/28064428-904e9a6a-662c-11e7-88ef-f343cf7566b2.png)


2. Select the 'Properties' tab. Click 'Static web hosting' and in the Index
Document box add the name of the html file that will be your home page.

	![Static web hosting](https://user-images.githubusercontent.com/22300773/28064534-f66d343c-662c-11e7-879e-66b39eebcf1c.png)

	Click Save. Note down the endpoint for your website. It will be of the form :

	`http://[bucket-name].s3-website-[region].amazonaws.com`

3. Select the permissions tab. We need to add a bucket policy that make the bucket content public so the website endpoint can show the website files.

	![permissions 1](https://user-images.githubusercontent.com/22300773/28064676-7e462bde-662d-11e7-8ee6-c78f7b0aded5.png)

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

	Replace 'example-bucket' with the name of your bucket.

4. Upload the index document and other files for your website.

	![upload](https://user-images.githubusercontent.com/22300773/28064703-98be1a44-662d-11e7-8218-2b9d8ff49d8d.png)

5. Test the website by entering the website URL in the browser!

	The uploading of files can be incorporated into your continuous integration process. **NB the names of the files need to be versioned to prevent the cached version being displayed after an update.**

### Notes

- Install Node.js on Ubuntu: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager#ubuntu-mint
- SSH Keep Alive: http://stackoverflow.com/questions/13228425/write-failed-broken-pipe
- More detail: http://unix.stackexchange.com/questions/34004/how-does-tcp-keepalive-work-in-ssh
- Simple Node Server: http://howtonode.org/hello-node
- EC2 Guide: http://aws.amazon.com/documentation/ec2/
- S3: Setting up a static website: http://docs.aws.amazon.com/AmazonS3/latest/dev/HostingWebsiteOnS3Setup.html
