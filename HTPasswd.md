# HTPasswd
OpenShift OAuth server can be configured to use many identity providers, like HTPasswd.

## Configuring the HTPasswd Identity Provider
Create new user and credential by HTPasswd:
```diff
# htpasswd -c -B -b /tmp/htpasswd user1 redhat123
Adding password for user user1
# htpasswd -b /tmp/htpasswd user1 RedHat123!
Updating password for user user1
# htpasswd -Bb /tmp/htpasswd user2 redhat123
Adding password for user user2

# cat /tmp/htpasswd 
user1:$apr1$spMCAuUD$Ppg8DnegsBG56eW90MwfC0
user2:$2y$05$5M6XoWioNX0F6GO2Y17u..KaVo9Qr7uzkT7qAKRlfm0Fi8zSj1T9y
```

Create secret accordingt to htpasswd file:
```diff
# oc create secret generic htpasswd-secret -n openshift-config --from-file htpasswd=/tmp/htpasswd 
secret/htpasswd-secret created
# oc get secret -n openshift-config |grep htpasswd-secret
htpasswd-secret                           Opaque                                1      8s
```

Export existing  OAuth cluster resource to a file in YAML format:
```diff
# oc get oauth cluster -o yaml
apiVersion: config.openshift.io/v1
kind: OAuth
metadata:
  creationTimestamp: "2022-01-05T00:20:14Z"
  generation: 2
  name: cluster
  resourceVersion: "374579"
  uid: a99d9535-6f75-47c8-bcf8-68b4bd8d1de1
spec:
  identityProviders:
  - htpasswd:
      fileData:
        name: oauth-htpasswd-admin
    mappingMethod: claim
    name: oauth-htpasswd-admin
    type: HTPasswd
```

Update oauth.yaml to include new secret and username:
```diff
# cat oauth.yaml 
apiVersion: config.openshift.io/v1
kind: OAuth
metadata:
  name: cluster
spec:
  identityProviders:
  - htpasswd:
      fileData:
        name: htpasswd-secret
    mappingMethod: claim
    name: oauth-htpasswd-user
    type: HTPasswd

# oc replace -f oauth.yaml 
oauth.config.openshift.io/cluster replaced

# oc get pod -n openshift-authentication 
NAME                               READY   STATUS    RESTARTS   AGE
oauth-openshift-5778d47cd8-vg6qn   1/1     Running   0          34s

```

Validate new user and credential:
```diff
# oc login -u user1 -p RedHat123! https://api.cnfdf06.ran.dfwt5g.lab:6443
Login successful.

You have access to 78 projects, the list has been suppressed. You can list all projects with 'oc projects'

Using project "default".

# oc whoami
user1

# oc get user
NAME    UID                                    FULL NAME   IDENTITIES
admin   e9ee39cf-5bf9-42a6-a040-18f3e08d9ca7               oauth-htpasswd-admin:admin
user1   de0c23ac-8f8e-43e9-8f48-cf739ee74e87               oauth-htpasswd-user:user1

# oc adm policy add-cluster-role-to-user cluster-admin user1
clusterrole.rbac.authorization.k8s.io/cluster-admin added: "user1"
```

Clean up:
```diff
# oc login -u admin -p RedHat123! https://api.cnfdf06.ran.dfwt5g.lab:6443
ogin successful.

You have access to 78 projects, the list has been suppressed. You can list all projects with 'oc projects'

Using project "default".

# oc whoami
admin

# oc delete user user1
user.user.openshift.io "user1" deleted

# oc delete identity oauth-htpasswd-user:user1
identity.user.openshift.io "oauth-htpasswd-user:user1" deleted

# oc delete secret htpasswd-secret -n openshift-config
secret "htpasswd-secret" deleted
```

