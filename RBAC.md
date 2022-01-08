## RBAC

- **Rule**    Allowed actions for objects or groups of objects.
- **Role**  	Sets of rules. Users and groups can be associated with multiple roles.
- **Binding** 	Assignment of users or groups to a role.

### Managing RBAC
Cluster role:
```diff
# oc adm policy add-cluster-role-to-user cluster-admin user1
clusterrole.rbac.authorization.k8s.io/cluster-admin added: "user1"

# oc adm policy remove-cluster-role-from-user cluster-admin user1

```
Normal role:
```diff
# oc policy add-role-to-user basic-user user2 -n test_project

```
