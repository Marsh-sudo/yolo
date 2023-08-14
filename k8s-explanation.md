## Yolo App on Kubernetes
This nodejs and mongodb project name yolo is deployed on Google Kubernetes Engine

RUNNING ON: 35.232.214.10:3005 

# Prerequisite
Make sure Minikube and kubectl are installed.

[kubectl installation](https://kubernetes.io/docs/tasks/tools/install-kubectl/)  
[Minikube installation](https://minikube.sigs.k8s.io/docs/start/)
Optional: Install Google cloud sdk

## Purpose
This Kubernetes manifest defines a Deployment and Service for a set of containers forming an application known as "yolo-app". The application consists of a client container, a backend container, and a MongoDB container, all managed by the Deployment, with the Service enabling external access. This cluster uses persistent volumes storage.

I created a deployment called `yolo-app-development` that uses labels `yolo-app` to manage is resource which we are going to look at a few.

- Created three replicas for our deployment this is to help in redudancy and prevent our app from crushing incase one of our pod dies.

# Containers Used:
Our deployment uses three docker images namely: Backend(`marshkelvin0/backend`), client(`marshkelvin/client`) and mongodb(`mongo`) images to create our containers.

- For my client container it uses port `3000` to receive http requests.

- For the backend container it uses port `5000` and mounts volumes to `/data/db` using the `volumeMounts` on our manifest. This is a type of storage known as the `persistent storage`

For our `env` variable we use variable `MONGO_ROOT_USERNAME` & `MONGO_ROOT_PASSWORD` to load our values from k8s `secrets` called `mongodb-secret`

## Services
In the same deployment manifest file `yoloapp-deployment.yaml` we configure a service that will be used to expose our deployment to the external world.

- Created a service called `yolo-app-service` that uses the label selector `yolo-app` and type `LoadBalancer` ports are set to 3005 for Port on the Service, target port is set to forward traffic to on the Pods

## Persistent Volumes
For this project I decided to use persistent volumes because when i decide to delete a pod that stores data in a database the database won't be deleted by the process this to ensure continuity of the storage. 

# yolo-persistent-volume.yaml
This is a persistent volume manifest file used to manage storage resources in Kubernetes independently from the Pods that use the storage.

- Name of the PVs is mongodb-pv
- in the spec i specify the storage capacity i need for my PVs which is `10Gi`

- For the access to the PVs we use Read and write which means the volume can be mounted as read-write by a single Node.

- It defines the StorageClass name `manual` for the PersistentVolume, which will be used to bind PersistentVolumeClaim requests to this PersistentVolume.

- For our project yolo to use PVs we need to create a Google Cloud `compute engine disk` named `yolo-app` from our Google cloud console in order for our PVs to be used externally.

Next we create a PersistentVolumeClaim. Pods use PersistentVolumeClaims to request physical storage. We create a PersistentVolumeClaim that requests a volume of at least three gibibytes that can provide read-write access for at most one Node at a time.

- We name our PVCs `mongodb-pvc` as it will be used in our mainfest deployment.

- One thing to note is at the `spec.resources` the storage and storageClassName should match with our Persistent Volume we created earlier.

## Mounting our PVC in our deployment
In our deployment `spec.volumes` we create volumes for our backend container which will inherit our PVC name we created in the `yolo-persistent-volume.yaml` in our case `mongodb-pvc`

- We also mount volume in backend container at mountPath `/data/db`

