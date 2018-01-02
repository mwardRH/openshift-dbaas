# OpenShift Version

## Deploy
```
oc login -u developer -pTest
oc new-project dbaas
oc project dbaas
```
```
oc create -f https://raw.githubusercontent.com/mwardRH/openshift-dbaas/master/cockroachdb-statefulset.yaml
oc create -f https://raw.githubusercontent.com/mwardRH/openshift-dbaas/master/cluster-init.yaml
```
```
oc get pods 
oc get services
```

```
oc run cockroachdb -it --image=cockroachdb/cockroach --rm --restart=Never -- sql --insecure --host=cockroachdb-public
```

## Test Section --
```
CREATE DATABASE bank;
CREATE TABLE bank.accounts (id INT PRIMARY KEY, balance DECIMAL);
INSERT INTO bank.accounts VALUES (1, 1000.50);
SELECT * FROM bank.accounts;

\q
```

## Monitoring

```
oc expose service cockroachdb

oc delete pod cockroachdb-2

kubectl scale statefulset cockroachdb --replicas=4

oc get pods
```

## END
