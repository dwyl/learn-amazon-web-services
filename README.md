Tutorials for AWS services

* [EC2](#ec2setup)
* [S3 Static Website Hosting](#s3-static-website-hosting)



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
