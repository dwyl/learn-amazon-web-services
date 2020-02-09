# Amazon Simple Email Service (SES) Setup

This guide takes you through setting up Amazon SES.


## Why?

All Software as a Service (SaaS) Products that create value send **`email`**. <br />
Even if it's just a reminder to pay a bill,
an **`email`** has to be sent _reliably_. <br />
Amazon's SES is a _reliable_ and _~~cheap~~_ _cheapest_ email service.

[@dwyl](https://github.com/dwyl) we _rely_ on **`email`** as our _primary_
[feedback loop](https://en.wikipedia.org/wiki/Feedback).
Without **`email`** we cannot _communicate_
with the people who are using our App.


## What?

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


## Who?

Anyone building an App
who wants to have control over their **`email`**.


## How?

Prerequisites:
This guide assumes you already have an AWS account.
Additionally, if you plan to follow along with
the _practical_ side of sending emails,
some familiarity with `JavaScript`
is advantageous but _not required_.

Follow the instructions in:
https://aws.amazon.com/getting-started/tutorials/send-an-email

### Setup

Login to your AWS account
and visit the SES endpoint:
https://console.aws.amazon.com/ses

![02-ses-home](https://user-images.githubusercontent.com/194400/73847617-925cde80-481e-11ea-9641-7f6e077952a2.png)

From the SES Home screen, click on "Email Addresses"

### 1. Verify an Email Address

In the "Email Addresses" menu, click on "Verify a New Email Address":

![aws-ses-add-email-address](https://user-images.githubusercontent.com/194400/74105174-59d93f80-4b53-11ea-8852-274b7d4f9028.png)


> **Note**:
In our case we had to _setup_ a **_free_ Zoho Mail** email account
for our domain in order to _receive_ the confirmation email. <br />
See:
[issues/35](https://github.com/dwyl/learn-amazon-web-services/issues/35)
for detailed steps on how to do this.
It takes around 5 mins.

#### 1.1 Verify _this_ Email Address

Once you have input the desired email address, click "Verify this Email Address"

![aws-ses-verify](https://user-images.githubusercontent.com/194400/74105673-ec7bdd80-4b57-11ea-8972-d59a0007305c.png)

You should see a messaging confirming that the verification email was sent:

![aws-ses-verification-sent](https://user-images.githubusercontent.com/194400/74105708-251bb700-4b58-11ea-832a-a820e6bc7a92.png)

### 2. Open Email and Click Verification Link

Open the email client associated with the email address you want to verify,
locate the verifcation email sent by AWS and click the link:

e.g: https://mail.zoho.eu

![aws-ses-zoho-mail-verification-email](https://user-images.githubusercontent.com/194400/74105742-5dbb9080-4b58-11ea-91ef-d112e8694acb.png)

Once you click the link you should see:

![aws-ses-congratulations](https://user-images.githubusercontent.com/194400/74105734-4bd9ed80-4b58-11ea-9a53-be6e3eabe192.png)

If you refresh the AWS you should see that the email address is verified:

![aws-ses-email-verified](https://user-images.githubusercontent.com/194400/74105797-efc39900-4b58-11ea-9d51-7e43c24780ee.png)


### 3. Send an Email

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


## _Way_ More Detail 💭
