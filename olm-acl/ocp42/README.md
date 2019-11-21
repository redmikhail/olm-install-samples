# Sample for RHPAM 7.5 operator deployment with OLM security policy 

## Manual deployment

### Executed as project admin user 

`oc new-project test1`

### Executed as cluster admin 

- Switch to the project - `oc project test1`
- Create OLM service account - `oc apply -f 01-create-sa.yml`
- Create role for OLM - `oc apply -f 02-sa-role.yml`
- Create SA/Role binding - `oc apply -f 03-sa-binding.yml`
- Create operatorgroup for single namespace while replacing environment variable for the target namespace - 
```
export TARGET_NAMESPACE="mm-test1";envsubst < 04-operator-group.yml | oc apply -f -
```

## Executed as project admin user  

- Switch to the project - `oc project test1`
- Create subscription - `oc apply -f 05-subscription.yml`
- Verify if subscription was successful 
```
oc get csv $(oc get subscription businessautomation-operator -n test1 -o jsonpath='{.status.installedCSV}') -n test1 -o jsonpath='{.status.phase}'
```
- Create trial application using deployed RHPAM operator - `oc apply -f 06-kie.yml`
