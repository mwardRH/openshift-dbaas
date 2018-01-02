# OpenShift Version

## Deploy

Log in and create OpenShift project
```
oc login -u developer -pTest
oc new-project dbaas
oc project dbaas
```
Create the Statefulset then initalize the Cockroachdb Cluster. 
```
oc create -f https://raw.githubusercontent.com/mwardRH/openshift-dbaas/master/sql-cockroachdb/cockroachdb-statefulset.yaml
oc create -f https://raw.githubusercontent.com/mwardRH/openshift-dbaas/master/sql-cockroachdb/cluster-init.yaml
```
Check the pods and services are status. 
```
oc get pods 
oc get services
```
Launch a new (one time) container for connecting to the Database. 
```
oc run cockroachdb -it --image=cockroachdb/cockroach --rm --restart=Never -- sql --insecure --host=cockroachdb-public
```

## Test Section --
Create a database and table. Insert data into the table. Quit. This should be done within the one time deploy container. 
```
CREATE DATABASE bank;
CREATE TABLE bank.accounts (id INT PRIMARY KEY, balance DECIMAL);
INSERT INTO bank.accounts VALUES (1, 1000.50);
SELECT * FROM bank.accounts;

\q
```

## Monitoring
Create a route and check the database health page. 
```
oc expose service cockroachdb
```
Delete a pod and show how the database is still able to service traffic. 
```
oc delete pod cockroachdb-2
```
Scale the statefulset from 3 to 5. 
```
kubectl scale statefulset cockroachdb --replicas=5
```
Check the status of your pods. 
```
oc get pods
```
This concludes the demo. Thank you for reading! 

