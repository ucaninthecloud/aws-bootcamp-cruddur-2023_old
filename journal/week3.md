# Week 3 — Decentralized Authentication

## Creating a new user pool

1. You need to create a new user pool.

<img src=assets/week3/2023-03-04-Amazon_Cognito_01.png width="70%">

<br>

2. Now the configuration should include **User name** and **email**

<img src=assets/week3/2023-03-04-Amazon_Cognito_02.png width="70%">

<br>

3. Leave the **Cognito defaults** and scroll down for MFA configuration (for this test, do not use MFA). Leave also the account recovery options to **Self-service account recovery** and let the default to be **Email** to minimize AWS cost due to SMS.

<img src=assets/week3/2023-03-04-Amazon_Cognito_03.png width="70%">

<br>
<img src=assets/week3/2023-03-04-Amazon_Cognito_04.png width="70%">

<br>

4. Leave **Self-service sign-up** and **Allow Cognito to automatically send messages**. Verify attributes changes by sending an email and select any required attributes. On below's screenshots we are selecting only **name** but later we may require **email** as well.

<img src=assets/week3/2023-03-04-Amazon_Cognito_05.png width="70%">

<br>
<img src=assets/week3/2023-03-04-Amazon_Cognito_06.png width="70%">

<br>

5. We are going to use the Cognito service until SES is setup for the project. 

<img src=assets/week3/2023-03-04-Amazon_Cognito_07.png width="70%">

<br>

6. Assign a user pool name. I setup as **cruddur_user_pool_clickops** but somehow I captured the screenshot as new_user_pool_name:

<img src=assets/week3/2023-03-04-Amazon_Cognito_08.png width="70%">

<br>

7. Leave the **Public client** when developing service applications and assign a meaningful **App client name**.

<img src=assets/week3/2023-03-04-Amazon_Cognito_09.png width="70%">

<br>

8. Submit to create and it is done!


## AWS Amplify

In order to use Cognito, we need to use the AWS Amplify Javascript library. This needs to be installed on the frontend-react-js directory (Similar to the standard **npm install**).

1. I added the installation to the [.gitpod.yml](../.gitpod.yml)

```yml
  - name: npm front-end
    init: |
      cd ./frontend-react-js
      npm install
      npm i aws-amplify --save
```

2. Edited the [App.js](../frontend-react-js/src/App.js) by adding the import statement:

```js
import { Amplify } from 'aws-amplify';
```

3. Added the Amplify configuration block after the last import statement:

```js
Amplify.configure({
  "AWS_PROJECT_REGION": process.env.REACT_APP_AWS_PROJECT_REGION,
  "aws_cognito_identity_pool_id": process.env.REACT_APP_AWS_COGNITO_IDENTITY_POOL_ID,
  "aws_cognito_region": process.env.REACT_APP_AWS_COGNITO_REGION,
  "aws_user_pools_id": process.env.REACT_APP_AWS_USER_POOLS_ID,
  "aws_user_pools_web_client_id": process.env.REACT_APP_CLIENT_ID,
  "oauth": {},
  Auth: {
    region: process.env.REACT_APP_AWS_PROJECT_REGION, //fixed this line to include _APP before _AWS
    userPoolId: process.env.REACT_APP_AWS_USER_POOLS_ID,
    userPoolWebClientId: process.env.REACT_APP_AWS_USER_POOLS_WEB_CLIENT_ID,
  }
});
```

4. From the above code, the REACT Environment variables were declared in the [docker-compose.yml](../docker-compose.yml) and also within Gitpod:

```yml
REACT_APP_AWS_PROJECT_REGION: "${AWS_DEFAULT_REGION}"
REACT_APP_AWS_COGNITO_REGION: "${AWS_DEFAULT_REGION}"
REACT_APP_AWS_USER_POOLS_ID: "${AWS_USER_POOLS_ID}"
REACT_APP_CLIENT_ID: "${APP_CLIENT_ID}"
```

<img src=assets/week3/2023-03-19-Gitpod_Variables.png width="70%">

<br>

##