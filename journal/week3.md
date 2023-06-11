# Week 3 — Decentralized Authentication

1 **Setup AWS Cognito User Pool**

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


**Step 4:**

+ In the Email provider section, change the selection to Send email with Cognito. After that, leave the default selection that comes with the option.

**Step 5:**

+ In the User pool name section, add a name of your choice; mine was cruddur-user-pool.
   Note: Your user pool name can't be changed once this user pool is created.
 
 ![userpool name selection](https://github.com/sarajagopal/aws-bootcamp-cruddur-2023/blob/main/journal/assets/coginto_config7.png)
 
+ Next to the Hosted authentication pages section, DON'T check the box that says Use the Cognito Hosted UI. We won't be using that feature for our project.
+ In the Initial app client section, select Public client as our App type.

+ Add a name for the App client name; mine was cruddur. After that, leave the rest of the default selections and click Next
![App Client name](https://github.com/sarajagopal/aws-bootcamp-cruddur-2023/blob/main/journal/assets/coginto_config8.png)

**Step 6:**

+ In this section, we will review all our configurations and then go ahead to Create user pool. After creation, you should get this success message.

![configuration completed](https://github.com/sarajagopal/aws-bootcamp-cruddur-2023/blob/main/journal/assets/coginto_config10.png)


2. **Configure Amazon Amplify**

Amazon Amplify is a complete solution that lets frontend web and mobile developers easily build, ship, and host full-stack applications on AWS, with the flexibility to leverage the breadth of AWS services as use cases evolve. Check out this link for better explanation of the service.

Let's go ahead and configure our Amazon Amplify.

+ In your Terminal, navigate to the frontend-react-js directory, and run this command:

```BASH

# navigate to frontend-react-js
cd frontend-react-js

# add aws-amplify library
npm i aws-amplify --save
# the --save flag saves the library to your package.json file 
```

+ Go into your frontend-react-js/src/App.js and add the following contents. Make sure to add it before the router configuration.

```BASH
// AWS Amplify
import { Amplify } from 'aws-amplify';

Amplify.configure({
  "AWS_PROJECT_REGION": process.env.REACT_APP_AWS_PROJECT_REGION,
  "aws_cognito_region": process.env.REACT_APP_AWS_COGNITO_REGION,
  "aws_user_pools_id": process.env.REACT_APP_AWS_USER_POOLS_ID,
  "aws_user_pools_web_client_id": process.env.REACT_APP_CLIENT_ID,
  "oauth": {},
  Auth: {
    // We are not using an Identity Pool
    // identityPoolId: process.env.REACT_APP_IDENTITY_POOL_ID, // REQUIRED - Amazon Cognito Identity Pool ID
    region: process.env.REACT_APP_AWS_PROJECT_REGION,           // REQUIRED - Amazon Cognito Region
    userPoolId: process.env.REACT_APP_AWS_USER_POOLS_ID,         // OPTIONAL - Amazon Cognito User Pool ID
    userPoolWebClientId: process.env.REACT_APP_AWS_USER_POOLS_WEB_CLIENT_ID,   // OPTIONAL - Amazon Cognito Web Client ID (26-char alphanumeric string)
  }
});
```

+ Now let's set our environment variables from the code mentioned above. Go into your docker-compose.yml file, and under the frontend-react-js service, add the following lines:

``` BASH
# AWS Amplify
REACT_APP_AWS_PROJECT_REGION: "${AWS_DEFAULT_REGION}"
REACT_APP_AWS_COGNITO_REGION: "${AWS_DEFAULT_REGION}"
REACT_APP_AWS_USER_POOLS_ID: "${REACT_APP_AWS_USER_POOLS_ID}"
REACT_APP_CLIENT_ID: "${REACT_APP_CLIENT_ID}"
```

+ Important - To view your user pool ID, select the user pool name from the AWS console. For the client ID, click into the pool you created and look for the App integration tab as shown in the image below. Then scroll all the way down to view your client ID

![Client ID](https://github.com/sarajagopal/aws-bootcamp-cruddur-2023/blob/main/journal/assets/coginto_config11.png)


+ In the frontend-react-js/src/pages/HomeFeedPage.js file, add the following lines of code:

``` JS
// AWS Amplify
import { Auth } from 'aws-amplify';

// DELETE THESE LINES
const checkAuth = async () => {
    console.log('checkAuth')
    // [TODO] Authenication
    if (Cookies.get('user.logged_in')) {
      setUser({
        display_name: Cookies.get('user.name'),
        handle: Cookies.get('user.username')
      })
    }
  };


// ADD THESE LINES 
// check if we are authenicated
const checkAuth = async () => {
  Auth.currentAuthenticatedUser({
    // Optional, By default is false. 
    // If set to true, this call will send a 
    // request to Cognito to get the latest user data
    bypassCache: false 
  })
  .then((user) => {
    console.log('user',user);
    return Auth.currentAuthenticatedUser()
  }).then((cognito_user) => {
      setUser({
        display_name: cognito_user.attributes.name,
        handle: cognito_user.attributes.preferred_username
      })
  })
  .catch((err) => console.log(err));
};
```

+ Now let's update our frontend-react-js/src/components/ProfileInfo.js file with the following contents: 

```JS

// DELETE THIS LINE 
import Cookies from 'js-cookie'

//ADD THIS LINE
// AWS Amplify
import { Auth } from 'aws-amplify';

// DELETE THESE LINES 
const signOut = async () => {
    console.log('signOut')
    // [TODO] Authenication
    Cookies.remove('user.logged_in')
    //Cookies.remove('user.name')
    //Cookies.remove('user.username')
    //Cookies.remove('user.email')
    //Cookies.remove('user.password')
    //Cookies.remove('user.confirmation_code')
    window.location.href = "/"
  }

// ADD THESE LINES 
const signOut = async () => {
  try {
      await Auth.signOut({ global: true });
      window.location.href = "/"
  } catch (error) {
      console.log('error signing out: ', error);
  }
}
```

