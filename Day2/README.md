# Day 2

## Lab - Deploying nginx in declarative style
```
cd ~
oc create deployment nginx --image=bitnami/nginx:1.18 -o yaml --dry-run=client
oc create deployment nginx --image=bitnami/nginx:1.18 -o yaml --dry-run=client > nginx-deploy.yml
oc apply -f nginx-deploy.yml
oc get deploy,rs,po
```

Expected output
![deploy](declarative-deploy1.png)
![deploy](declarative-deploy2.png)
![deploy](declarative-deploy3.png)

## Lab - Generating declarative manifest scripts for clusterip, nodeport and loadbalancer services
```
oc get deploy
oc expose deploy/nginx --type=ClusterIP --port=8080 -o yaml --dry-run=client
oc expose deploy/nginx --type=ClusterIP --port=8080 -o yaml --dry-run=client > nginx-clusterip-svc.yml
oc expose deploy/nginx --type=NodePort --port=8080 -o yaml --dry-run=client > nginx-nodeport-svc.yml
oc expose deploy/nginx --type=LoadBalancer --port=8080 -o yaml --dry-run=client > nginx-lb-svc.yml
ls
```

Expected output
![service](service.png)

## Lab - Creating clusterip internal service in declarative style
```
oc expose deploy/nginx --type=ClusterIP --port=8080 -o yaml --dry-run=client > nginx-clusterip-svc.yml
ls -l
oc apply -f nginx-clusterip-svc.yml
oc get svc
```

You can also delete the service in declarative style
```
oc delete -f nginx-clusterip-svc.yml
oc get svc
```
Expected output
![service](service1.png)


## Lab - Creating nodeport external service in declarative style
```
oc expose deploy/nginx --type=NodePort --port=8080 -o yaml --dry-run=client > nginx-nodeport-svc.yml
ls -l
oc apply -f nginx-nodeport-svc.yml
oc get svc
```

You can also delete the service in declarative style
```
oc delete -f nginx-nodeport-svc.yml
oc get svc
```
Expected output
![service](service1.png)


## Lab - Creating loadbalancer external service in declarative style
```
oc expose deploy/nginx --type=LoadBalancer --port=8080 -o yaml --dry-run=client > nginx-lb-svc.yml
ls -l
oc apply -f nginx-lb-svc.yml
oc get svc
```

You can also delete the service in declarative style
```
oc delete -f nginx-lb-svc.yml
oc get svc
```

Expected output
![service](service1.png)


## Lab - Scaling up/down deployment in declarative style

Scale up the nginx deployment from 3 pods to 5 pods.  You need to edit the nginx-deploy.yml file, replace the replicas from 3 to 5, save and apply as shown below.
```
cd ~/openshift-3june-2024
git pull
cd Day2/declarative-manifest-scripts
cat nginx-deploy.yml
oc get po
oc apply -f nginx-deploy.yml
oc get po
```

Expected output
![scale-up](scale-up1.png)
![scale-up](scale-up.png)


Scale down the nginx deployment from 5 pods to 3 pods.  You need to edit the nginx-deploy.yml file, replace the replicas from 5 to 3, save and apply as shown below.
```
cd ~/openshift-3june-2024
git pull
cd Day2/declarative-manifest-scripts
cat nginx-deploy.yml
oc get po
oc apply -f nginx-deploy.yml
oc get po
```

Expected output
![scale-down](scaled-down1.png)
![scale-down](scale-down.png)

## Lab - Rolling update to upgrade your live application from one version to other without any downtime
You need to edit nginx-deploy.yml and update the image version from 1.18 to 1.19, save it and apply
```
cd ~/openshift-3june-2024
git pull
cd Day2/declarative-manifest-scripts
cat nginx-deploy.yml
oc apply -f nginx-deploy.yml
```

To check the status of rolling update
```
oc rollout status deploy/nginx
oc rollout history deploy/nginx
```

To rollback to previous version
```
oc rollout undo deploy/nginx
```

Expected output
![rolling-update](rolling-update1.png)
![rolling-update](rolling-update2.png)
![rolling-update](rolling-update3.png)
![rolling-update](rolling-update4.png)
![rolling-update](rolling-update5.png)

## Lab - Creating a replicaset in declarative style without deployment
First let's delete any deployment running
```
cd ~/openshift-3june-2024
git pull
cd Day2/declarative-manifest-scripts
oc delete -f nginx-deploy.yml
```

Let's create the replicaset
```
cd ~/openshift-3june-2024
cd Day2/declarative-manifest-scripts
cat nginx-rs.yml
oc apply -f nginx-rs
oc get deploy,rs,po
```

Once you are done, you may delete the replicaset declaratively
```
cd ~/openshift-3june-2024
cd Day2/declarative-manifest-scripts
oc delete -f nginx-rs
oc get rs,po
```

Expected output
![replicaset](rs.png)
![replicaset](rs1.png)
![replicaset](rs2.png)


## Lab - Creating a pod in declaratively style without replicaset and deployment
```
cd ~/openshift-3june-2024
git pull
cd Day2/declarative-manifest-scripts
cat pod.yml
oc apply -f pod.yml
oc get pod
```
Expected output
![pod](pod.png)

Once you are done with this exercise, you may delete the pod declaratively as shown below
```
cd ~/openshift-3june-2024
cd Day2/declarative-manifest-scripts
oc delete -f pod.yml
oc get pod
```

Expected output
![pod](pod1.png)

## Lab - Creating a route to expose the clusterip service with a convenient public url
As we discussed, the nodeport service is neither end-user friendly nor developer friendly. Also it forces us to open ports in the firewall it leads to security issues.

Route is the solution to the nodeport service issues.  

Let's create a route
```
cd ~/openshift-3june-2024
git pull
cd Day2/declarative-manifest-scripts
ls -l
oc apply -f nginx-deploy.yml
oc apply -f nginx-clusterip-svc.yml
oc expose svc/nginx -o yaml --dry-run=client
oc expose svc/nginx -o yaml --dry-run=client > nginx-route.yml
oc apply -f nginx-route.yml

oc get route
```

You may access the route as shown below
```
curl http://nginx-jegan.apps.ocp4.tektutor.org.labs
```

Expected output
![route](route.png)

Once you are done with this exercise, you may delete the resources as shown below
```
cd ~/openshift-3june-2024
cd Day2/declarative-manifest-scripts
ls -l

oc get deploy,svc,route
oc delete -f nginx-route.yml
oc delete -f nginx-clusterip-svc.yml
oc delete -f nginx-deploy.yml
oc get deploy,svc,route
```

Expected output
![route](route1.png)

## Lab - Deploying an application in declarative style from Webconsole in developer context
![webconsole](webconsole1.png)
![webconsole](webconsole2.png)
![webconsole](webconsole3.png)
![webconsole](webconsole4.png)
![webconsole](webconsole5.png)
![webconsole](webconsole6.png)
![webconsole](webconsole7.png)
![webconsole](webconsole8.png)
![webconsole](webconsole9.png)
![webconsole](webconsole10.png)

## Lab - Create a clusterip service in declarative style from webconsole in developer context
![webconsole](web-service1.png)
![webconsole](web-service2.png)
![webconsole](web-service3.png)
![webconsole](web-service4.png)
![webconsole](web-service5.png)
![webconsole](web-service6.png)
![webconsole](web-service7.png)

## Lab - Create a route in declarative style from webconsole in developer context
![webconsole](web-route1.png)
![webconsole](web-route2.png)
![webconsole](web-route3.png)
![webconsole](web-route4.png)
![webconsole](web-route5.png)
![webconsole](web-route6.png)
![webconsole](web-route7.png)
![webconsole](web-route8.png)

## Info - What is Persistent Volume (PV) ?
<pre>
- is an external storage used by application deployed in Kubernetes/Openshift
- Persistent volumes are created in the cluster scope, to allow application running from any project namespace to claim them
- Administrators can provision the Persistent Volume from various storage solutions manually or dynamically
  - NFS
  - AWS EBS
  - AWS S3
  - Azure Storage
- When Persistent Volumes are created by Administrators, they will have define
  - Mandatory field - size/capacity of the storage
  - Mandatory field - access mode ( ReadWriteOnce, ReadWriteMany, etc., )
  - Storage Class - optional
  - labels - optional but recommended
</pre>

## Info - What is Persistent Volume Claim (PVC) ?
<pre>
- Any application that requires external storage has to request for external storage by creating a Persistent volume claim
- Unless the Pod that expects the external get a PV matching the PVC criteria is found, the Pod will remain in the Pending state only 
- The PVC has mention certain fields
  - size of the storage (mandatory)
  - access mode (mandatory)
  - storage class (optional)
  - label selector ( optional )
</pre>

## Info - What is Storage Class ?
<pre>
- Administrators can also provision the Persistent Volume(PV) on demand in a dynamic fashion by creating storage class
- For every type of storage, we need to create a separate storage class
</pre>


## Lab - Deploying mariadb database server using its internal storage to store database, tables, etc

```
cd ~/openshift-3june-2024
git pull
cd Day2/persistent-volume
oc create deployment mariadb --image=bitnami/nginx:latest -o yaml --dry-run=client
oc create deployment mariadb --image=bitnami/nginx:latest -o yaml --dry-run=client > mariadb-deploy.yml
cat mariadb-deploy.yml

oc apply -f mariadb-deploy.yml
oc get deploy,po
```

Getting inside the mariadb pod container shell. When prompts for password type 'root@123' as password without quotes.
```
oc rsh deploy/mariadb
hostname
hostname -i
mysql -u root -p
SHOW DATABASES;

CREATE DATABASE tektutor;

USE tektutor;

CREATE TABLE training ( id INT NOT NULL, name VARCHAR(250) NOT NULL, duration VARCHAR(250) NOT NULL, PRIMARY KEY(id));

INSERT INTO training VALUES ( 1, "DevOps", "5 Days" );
SELECT * FROM training;
```

Expected output
![mariadb](mariadb1.png)
![mariadb](mariadb2.png)
![mariadb](mariadb3.png)
![mariadb](mariadb4.png)
![mariadb](mariadb5.png)
![mariadb](mariadb6.png)
![mariadb](mariadb7.png)
![mariadb](mariadb8.png)
![mariadb](mariadb9.png)
![mariadb](mariadb10.png)
![mariadb](mariadb11.png)
![mariadb](mariadb12.png)

## Lab - Mariadb with Persistent Volume
Let's delete the existing mariadb deployment
```
oc delete deploy/mariadb
```

Now, let's deploy mariadb with PV and PVC
```
cd ~/openshift-3june-2024
git pull
cd Day2/persistent-volume
ls -l
oc apply -f mariadb-pv.yml
oc apply -f mariadb-pvc.yml
oc apply -f mariadb-deploy.yml

oc get po,pv,pvc
```

Let's get inside the mariadb pod shell, when prompts for password type 'root@123' without quotes
```
oc rsh deploy/mariadb
mysql -u root -p
SHOW DATABASES;
CREATE DATABASE tektutor;
USE tektutor;
CREATE TABLE training ( id INT NOT NULL, name VARCHAR(250) NOT NULL, duration VARCHAR(250) NOT NULL, PRIMARY KEY(id));

INSERT INTO training VALUES ( 1, "Openshift", "5 days" );
SELECT * FROM training;
exit
exit
```

Now, let's delete the mariadb pod, openshift will automatically redepoloy a new mariadb pod.
Let's get inside the new mariadb pod
```
oc rsh deploy/mariadb
mysql -u root -p
SHOW DATABASES;
USE tektutor;
SELECT * FROM training;
```
You are expected to see the tektutor database and training table intact with all the records we inserted via the old mariadb pod we deleted. This is made possible with the help of external Persistent Volume.

Expected output
![mariadb](maria1.png)
![mariadb](maria2.png)
![mariadb](maria3.png)
![mariadb](maria4.png)
![mariadb](maria5.png)
![mariadb](maria6.png)
![mariadb](maria7.png)
![mariadb](maria9.png)
![mariadb](maria10.png)
![mariadb](maria11.png)

Once you are done with this exercise, you may delete the mariadb and related pv and pvc to freeup resources.
```
cd ~/openshift-3june-2024
cd Day2/persistent-volume

oc delete -f mariadb-deploy.yml
oc delete -f mariadb-pvc.yml
oc delete -f mariadb-pv.yml
```
If you noticed above, we have to delete the resources in the reverse order.  While deploying mariadb, we would have created the pv first, followed by pvc and then the deployment at the last, while deleting we need to follow reverse order to save time.

Expected output
![mariadb](maria8.png)
