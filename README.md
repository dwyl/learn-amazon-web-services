EC2Setup
========

Documenting/sharing how I get an EC2 instance up and running.

- - - 

If you don't already have an AWS account you will need to register for one:



### Connecting to your instance via SSH (Terminal/Console)

If you only have one SSH Key on your computer you can log into your EC2 Instance
using a command:


``` terminal
ssh -i ~/.ssh/nelsonic.pem ubuntu@ec2-54-229-220-192.eu-west-1.compute.amazonaws.com
```


### Notes

- https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager#ubuntu-mint



to keep the connection to the server alive,
open your ssh config file

vi ~/.ssh/config

and add the follwing lines:

