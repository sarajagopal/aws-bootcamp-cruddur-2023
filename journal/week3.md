# Week 3 — Decentralized Authentication

**Setup AWS Cognito User Pool**

Amazon Cognito provides authentication, authorization, and user management for your web and mobile apps. The users can sign in directly with a username and password or through a third party such as Facebook, Amazon, Google, or Apple. We will be working with one of its components known as user pool.

User pools are simply user directories that provide sign-up and sign-in options for your app users. Check out this [link](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html) for better explanation of the service.

**Step 1:**

We will be using the AWS console to set up our Cognito User Pool.  
+ login to your AWS account . select the service Amazon Cognito.
+ In the Provider types section, select Cognito user pool .
+ Now to the Cognito user pool sign-in options, make sure to select only the User name and Email attributes. 
+ DON'T check any box for the User name requirements. Click Next
![create user Pool](https://github.com/sarajagopal/aws-bootcamp-cruddur-2023/blob/main/journal/assets/coginto_config1.png)

**Step 2:**

In the Password policy section, we will be using the Cognito defaults for our project, but you can set a custom one if you want.
Next to the Multi-factor authentication section. We won't be using any MFA because this service is not included in the free tier. But, if you want to add an MFA, go over the pricing before implementation.

![config Security Requirment](https://github.com/sarajagopal/aws-bootcamp-cruddur-2023/blob/main/journal/assets/coginto_config2.png)

+ In the User account recovery section, check the box that says Enable self-service account recovery. This feature enables users to be able to reset their password whenever they want.

+ Now select the Email only option. We are using this feature because it is free. If you want to use the SMS option, check out the pricing. Click Next

![User Account Recovery](https://github.com/sarajagopal/aws-bootcamp-cruddur-2023/blob/main/journal/assets/coginto_config3.png)

**Step 3:**

+ In the Self-service sign-up section, make sure you check the box that says  Enable self-registration.

![Self-registration](https://github.com/sarajagopal/aws-bootcamp-cruddur-2023/blob/main/journal/assets/coginto_config4.png)

+ Next to the Attribute verification and user account confirmation section. Make sure to leave the default selections. See the image below:

![Attribute Verification](https://github.com/sarajagopal/aws-bootcamp-cruddur-2023/blob/main/journal/assets/coginto_config5.png)


+ In the Required attributes section, add some additional required attributes - name and preferred_username.
Note: Required attributes can't be changed once this user pool has been created.

 The custom attributes are optional to set since we will be using a database to store our data. Click Next

![Customize attributes](https://github.com/sarajagopal/aws-bootcamp-cruddur-2023/blob/main/journal/assets/coginto_config6.png)


