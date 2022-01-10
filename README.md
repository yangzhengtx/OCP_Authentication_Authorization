# OAuth

Authentication and authorization are the two security layers responsible for enabling user interaction with the cluster. When a user makes a request to the API, the API associates the user with the request. The authentication layer authenticates the user. Upon successful authentication, the authorization layer decides to either honor or reject the API request. The authorization layer uses role-based access control (RBAC) policies to determine user privileges.

The OpenShift API has two methods for authenticating requests:
- OAuth Access Tokens
- X.509 Client Certificates

The OpenShift Container Platform provides the Authentication operator, which runs an OAuth server. The OAuth server provides OAuth access tokens to users when they attempt to authenticate to the API. An identity provider must be configured and available to the OAuth server. The OAuth server uses an identity provider to validate the identity of the requester. The server reconciles the user with the identity and creates the OAuth access token for the user. OpenShift automatically creates identity and user resources after a successful login.

