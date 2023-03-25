# Week 3 — Decentralized Authentication

1 **Setup AWS Cognito User Pool**

Amazon Cognito provides authentication, authorization, and user management for your web and mobile apps. The users can sign in directly with a username and password or through a third party such as Facebook, Amazon, Google, or Apple. We will be working with one of its components known as user pool.

User pools are simply user directories that provide sign-up and sign-in options for your app users. Check out this [link](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html) for better explanation of the service.

We will be using the AWS console to set up our Cognito User Pool.  
+ login to your AWS account . select the service Amazon Cognito.
+ In the Provider types section, select Cognito user pool as shown below:
+ Now to the Cognito user pool sign-in options, make sure to select only the User name and Email attributes. 
+ DON'T check any box for the User name requirements. Click Next
