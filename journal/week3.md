# Week 3 — Decentralized Authentication
Reference
![Link to Decentralised authentication](https://www.loginradius.com/blog/identity/what-is-decentralized-authentication/)
Decentralized authentication simply means that there is no central authority needed to verify your identity, i.e., decentralized identifiers. DIDs (Decentralized Identifiers) are a special type of identifier that allows for decentralized, verified digital identification. A DID is any subject identified by the DID's controller (e.g., a person, organization, thing, data model, abstract entity, etc.).
DIDs, unlike traditional federated identifiers, are designed to be independent of centralized registries, identity providers, and certificate authorities.
### Sign into AWS and configure cognito

### Creating user pool (user group)
Sign into AWS Console and open cognito, we will use user pools(but not the difference btn user pools and federated identities-allows third party IP)
Choose ``` username ``` and ``` email ``` as sign in option and then Next page
Choose the ``` password policy ``` type in our case we would choose the ``` defaults ```
In the Multi factor authenticator, choose ```no MFA ```
Under user account recovery choose only ``` Email ``` because of spend and allow users to ``` Enable self-service account recovery ```
Next, configure new users experience , allow users at sign up to ``` enable self registration ```
Attribute for verification should be set to ``` Email ```  and verify with ``` Email ``` becuase of cost
Required attributes should be set to ``` name ``` only and then Next Page
Configure message delivery with ``` Send email with Cognito ``` then choose AWS default email given and then Next
Now give user pool a name  with ``` cruddur-user-pool ``` and uncheck ``` Use the Cognito Hosted UI ``` and app type as ```public ``` and then name as ``` cruddur ``` and client secret as ``` Don't generate a client secret ``` an then Next Page 
Now click on ``` create user pool ```

### Install AWS Amplify(similar to GCP firebase) as a depency to login
locate your frontend and install aws amplify with this ``` npm i aws-amplify --save ```
