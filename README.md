For users to interact with OpenShift Container Platform, they must first authenticate to the cluster. The authentication layer identifies the user associated with requests to the OpenShift Container Platform API. The authorization layer then uses information about the requesting user to determine if the request is allowed.

Authentication and authorization are the two security layers responsible for enabling user interaction with the cluster. When a user makes a request to the API, the API associates the user with the request. The authentication layer authenticates the user. Upon successful authentication, the authorization layer decides to either honor or reject the API request. The authorization layer uses role-based access control (RBAC) policies to determine user privileges.

# Authentication
- Configuring an identity provider:
- Configuring the internal OAuth Server:


The OpenShift API has two methods for authenticating requests:
- OAuth Access Tokens
- X.509 Client Certificates

The OpenShift Container Platform provides the Authentication operator, which runs an OAuth server. The OAuth server provides OAuth access tokens to users when they attempt to authenticate to the API. An identity provider must be configured and available to the OAuth server. The OAuth server uses an identity provider to validate the identity of the requester. The server reconciles the user with the identity and creates the OAuth access token for the user. OpenShift automatically creates identity and user resources after a successful login.



# Authorization
- RBAC: Rules,  Local/Cluster Roles and Bindings. For user/group/SA.
- SCC for Pod


Reference:
https://docs.openshift.com/container-platform/4.10/authentication/index.html
