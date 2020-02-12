# Amazon Simple Email Service (SES) Setup 🔧

This guide takes you through setting up
a verified email address on Amazon SES
and escaping the "sandbox".


## Why? 🤷

All Software as a Service (SaaS) Products that create value send **`email`**. <br />
Even if it's just a reminder to pay a bill,
an **`email`** has to be sent _reliably_. <br />
Amazon's SES is a _reliable_, full featured
and _~~cheap~~_ _cheapest_ email service.

[@dwyl](https://github.com/dwyl) we _rely_ on **`email`** as our _primary_
[feedback loop](https://en.wikipedia.org/wiki/Feedback). <br />
Without **`email`** we cannot
[_communicate_](https://github.com/dwyl/app/issues/267)
with the people who are using our App.


## What? 💭

Amazon Web Services (AWS) Simple Email Service (SES)
lets you **send** and **_receive_ email**. <br />
Additionally, for more advanced users SES
has
[***notifications***](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/notification-contents.html)
with _insights_
about the email messages you send
e.g: **`delivery`**, **`bounce`** or **`complaint`**.

> We are not going to explore notifications in _this_ setup guide,
but we have a Lambda Function: https://github.com/dwyl/aws-ses-lambda
that you should consider exploring if you want a _turn-key_
solution to your AWS SES needs.


## Who? 👤

Anyone building an App
who wants to have control over their **`email`**.

This guide follows and enhances the official AWS instructions:
https://aws.amazon.com/getting-started/tutorials/send-an-email

## How? ✅

Prerequisites: <br />
This guide assumes you already have an AWS account. <br />
Additionally, if you plan to follow along with
the _practical_ side of sending emails,
some familiarity with `JavaScript`
is advantageous but _not required_.

### Setup 🛠

Login to your AWS account
and visit the SES endpoint:
https://console.aws.amazon.com/ses

![02-ses-home](https://user-images.githubusercontent.com/194400/73847617-925cde80-481e-11ea-9641-7f6e077952a2.png)

From the SES Home screen, click on "Email Addresses"

### 1. Verify an Email Address ✉️

In the "Email Addresses" menu, click on "Verify a New Email Address":

![aws-ses-add-email-address](https://user-images.githubusercontent.com/194400/74105174-59d93f80-4b53-11ea-8852-274b7d4f9028.png)


> **Note**:
In our case we had to _setup_ a **_free_ Zoho Mail** email account
for our domain in order to _receive_ the confirmation email.
See:
[issues/35](https://github.com/dwyl/learn-amazon-web-services/issues/35)
for detailed steps on how to do this.
It takes around **5 mins**.

#### 1.1 Verify _this_ Email Address

Once you have input the desired email address, click "Verify this Email Address"

![aws-ses-verify](https://user-images.githubusercontent.com/194400/74105673-ec7bdd80-4b57-11ea-8972-d59a0007305c.png)

You should see a messaging confirming that the verification email was sent:

![aws-ses-verification-sent](https://user-images.githubusercontent.com/194400/74105708-251bb700-4b58-11ea-832a-a820e6bc7a92.png)

### 2. Open Email and Click Verification Link 🔗

Open the email client associated with the email address you want to verify,
locate the verifcation email sent by AWS and click the link:

e.g: https://mail.zoho.eu

![aws-ses-zoho-mail-verification-email](https://user-images.githubusercontent.com/194400/74105742-5dbb9080-4b58-11ea-91ef-d112e8694acb.png)

Once you click the link you should see:

![aws-ses-congratulations](https://user-images.githubusercontent.com/194400/74105734-4bd9ed80-4b58-11ea-9a53-be6e3eabe192.png)

If you refresh the AWS you should see that the email address is verified:

![aws-ses-email-verified](https://user-images.githubusercontent.com/194400/74105797-efc39900-4b58-11ea-9d51-7e43c24780ee.png)


### 3. Send a Test Email 📤  

In the Amazon SES console,
select the radio button to the left of the email address you just verified,
and then click on "Send a Test Email":

![aws-ses-send-test-1](https://user-images.githubusercontent.com/194400/74106498-7b402880-4b5f-11ea-8b81-1cd17ac95e0a.png)


#### 3.1 Send _Formatted_ Email

Compose and send a ***Formatted*** email:

![aws-ses-send-test-compose](https://user-images.githubusercontent.com/194400/74106507-84c99080-4b5f-11ea-908a-63b64eac57e7.png)

In your email you should see:

<img width="518" alt="image" src="https://user-images.githubusercontent.com/194400/74106642-73cd4f00-4b60-11ea-98d7-06d455acce47.png">


#### 3.2 Send _Raw_ Email

Next try sending a ***Raw*** email.

![aws-ses-send-raw-email](https://user-images.githubusercontent.com/194400/74106567-fb668e00-4b5f-11ea-9174-e85d1ef5f082.png)


Paste the following text:

```html
Subject: Amazon SES Test
MIME-Version: 1.0
Content-Type: text/html

<!DOCTYPE html>
<html>
<body>
  <h1>You have successfully sent an email using Amazon SES!</h1>
  <p>For more information about Amazon SES, see the
    <a href="http://docs.aws.amazon.com/ses/latest/DeveloperGuide/Welcome.html">
      Amazon SES Developer Guide
    </a>.
  </p>
</body>
</html>
```

In your email, you should see:

<img width="508" alt="image" src="https://user-images.githubusercontent.com/194400/74106663-8fd0f080-4b60-11ea-9d00-fce76a215dc9.png">



### 4. Escape the Sandbox 👮

At this point you have _successfully_ setup AWS SES,
but you can only _send_ emails to verified email addresses,
which is not much _use_ beyond testing.

In order to send email to _any_ address,
you need to request that AWS remove your account from the "sandbox":
https://docs.aws.amazon.com/ses/latest/DeveloperGuide/request-production-access.html

In the AWS SES interface, click on the Support selector in the top-right and select "Support Center":
![aws-ses-support](https://user-images.githubusercontent.com/194400/74107680-9cf2dd00-4b6a-11ea-91e1-6062641f4197.png)

Once in the AWS Support interface, click on "create support case":
![aws-ses-create-case](https://user-images.githubusercontent.com/194400/74107723-ea6f4a00-4b6a-11ea-9d8a-c795e68c31cf.png)

In the case creation interface, select the "Service limit increase" option:
![aws-case-increase-limits](https://user-images.githubusercontent.com/194400/74107733-fb1fc000-4b6a-11ea-8551-ae312c01b3cf.png)

Input the relevant data and then click on "Submit":

![request](https://user-images.githubusercontent.com/194400/74107798-987af400-4b6b-11ea-9ea5-ee32bc695740.png)

![aws-ses-click-submit](https://user-images.githubusercontent.com/194400/74107757-38844d80-4b6b-11ea-82c5-5c42f51c5c34.png)

Once you submit the case you see a summary similar to this:

![aws-ses-case-details](https://user-images.githubusercontent.com/194400/74108011-813d0600-4b6d-11ea-964c-aa193426beb2.png)

A day later ... you should expect something similar to this:

![aws-ses-setup-out-of-sandbox](https://user-images.githubusercontent.com/194400/74197166-bf622480-4c56-11ea-80bf-60ad8c1efb55.png)


### 5. Send an Email! 💡

Now that you have verified your email address
and you are out of the "sandbox",
you can send email!

Try following the instructions in:
https://github.com/dwyl/sendemail



### 6. Create IAM Policy to Send Email from a Lambda Function

If you want to send an email from an AWS Lambda function,
there are a few extra steps to setup an IAM policy to allow this.

On the IAM policies homepage, select **Policies** and then **Create policy**:
https://console.aws.amazon.com/iam/home?region=eu-west-1#/policies
![aws-iam-lambdases-01-home](https://user-images.githubusercontent.com/194400/74332050-e0c52c80-4d8c-11ea-8a8f-db8f617e120c.png)

Select the **JSON** tab and paste the `json` from the instructions,
then click **Review policy**
https://aws.amazon.com/premiumsupport/knowledge-center/lambda-send-email-ses
![aws-iam-lambdases-02-create](https://user-images.githubusercontent.com/194400/74332213-2bdf3f80-4d8d-11ea-87f4-7858b22035f4.png)

Review the policy and _name_ it e.g: **LambdaSES**
![aws-iam-lambdases-03-name](https://user-images.githubusercontent.com/194400/74332428-97c1a800-4d8d-11ea-8be3-538126d6f407.png)
Cant share a link in this description, so just google for
**How do I send email using Lambda and Amazon SES**

Once you click on **Create policy**
you should see a confirmation **`LambdaSES has been created.`**:
![aws-iam-lambdases-04-confirm](https://user-images.githubusercontent.com/194400/74332577-f424c780-4d8d-11ea-8a10-cda8657e1ef3.png)

Next we need to _attach_ the policy to the _existing_ role.
Search for the policy, select it and
then click on the **Policy actions** selector:
![aws-iam-lambdases-05-policy-action](https://user-images.githubusercontent.com/194400/74332683-3221eb80-4d8e-11ea-8d57-8f2bf1cc3daa.png)

Attach the policy to the `LambdaExecRole`:
![aws-iam-lambdases-06-policy-attach-to-role](https://user-images.githubusercontent.com/194400/74332756-5d0c3f80-4d8e-11ea-82e2-44814d9c4ea4.png)

Once you click the **Attach policy** button,
you should see a confirmation similar to this:
![aws-iam-lambdases-07-policy-attched-confirm](https://user-images.githubusercontent.com/194400/74332856-9e9cea80-4d8e-11ea-8b01-2afd05759c2c.png)


This will allow you to send email from a Lambda function e.g:
![aws-ses-lambda-test-send](https://user-images.githubusercontent.com/194400/74333404-cc366380-4d8f-11ea-8470-95de50ff296f.png)

Email received:
![aws-ses-lambda-result-email-received](https://user-images.githubusercontent.com/194400/74333626-38b16280-4d90-11ea-9878-05c5e93d3fd3.png)





<br /><hr /><br /><br />

## tl;dr > Why Use SES? 💰

### AWS Pricing (Lambda + SES) _Per Email_: $0.0001004

Each email sent has 2 "_parts_" with corresponding costs.

#### Lambda Price [$0.0000002 + $0.000000208](http://www.wolframalpha.com/input/?i=$0.0000002+%2B+$0.000000208) = $0.00004 (_per request/execution_)

https://aws.amazon.com/lambda/pricing/

![lambda pricing](https://cloud.githubusercontent.com/assets/194400/22722867/c3cedb64-edb1-11e6-97b6-8075315b5726.png)


Lambda Pricing is broken down into two components:
+ $0.0000002 per request (_execution cost regardless of duration/memory used_)
+ $0.000000208 per 100ms (_execution time_)


#### SES Price [$0.10 / 1000](http://www.wolframalpha.com/input/?i=$0.1%2F1000) = $0.0001

https://aws.amazon.com/ses/pricing/
![AWS SES Pricing](https://cloud.githubusercontent.com/assets/194400/22722910/1f50065c-edb2-11e6-9b91-fe9b75ee973b.png)


<!--
#### API Gateway [$3.50 / 1,000,000](http://www.wolframalpha.com/input/?i=$3.50+%2F+1000000) = $0.00035 (_per request_)

The API Gateway is _useful_ in the "_Serverless_" context.
e.g: if we wanted the ability to send an email directly from a client-side app
without going through our application server.

https://aws.amazon.com/api-gateway/pricing/
![API Gateway Pricing](https://cloud.githubusercontent.com/assets/194400/22722312/fbe646b2-edad-11e6-8967-f375be10401b.png)

> **Note**: We _decided_ to ***remove*** the API Gateway from our solution
because it added no value (_actually it adds latency!_)
to this application (_we aren't using caching or request throttling_)
and contributed the _vast majority of the **cost**_!!
-->

### Conclusion

We need to make our total (_incremental_) cost of running
our "Email Solution" _significantly_ cheaper,
while delivering comparable features to other providers.
That's why we need to use _both_ SES _and_ Lambda
to both _send_ emails and track notifications,
hence: https://github.com/dwyl/aws-ses-lambda

For _all_ companies/teams using AWS sending up to **65,000 Emails a Month**
will be **_Completely_ Free**.
(_covered by the free usage tier for the first 12 months_).

<!--
### Longer Term

Our ***long-term plan*** is to run ***all*** our own infrastructure.
see: https://github.com/dwyl/time/issues/153
Not only is it _cheaper_ to run our own hardware than to _burn_ money on AWS,
but we get the added benefit of being **_fully_ in control** of where
our data is stored and encrypting all communication at all times.
For the next few months we will be using AWS because it's "fit for purpose",
and by building this as a Lambda that uses SES and exposes a simple API,
we can _easily_ substitute it later when we move to our own infra.
-->
