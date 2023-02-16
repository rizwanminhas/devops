Mongo Express (browser) -> Mongo Express (external service) -> MongoExpress (pod) -> MongoDB (internal service) -> MongoDB (pod)

1. Create secrets for mongo username and password. `kubectl apply -f mongo-secrets.yaml` and then to check to `kubectl get secret`
   1. To encode some text in base 64 execute `echo -n 'username' | base64`
2. Create mongo-deployment.yaml `kubectl apply -f  mongo.yaml`, check with `kubectl get all` and `kubectl get pod` and `kubectl get pod --watch` and `kubectl describe pod POD_NAME`.
3. Create internal service `kubectl apply -f mongo.yaml` check with `kubectl get service` and `kubectl describe service mongodb-service` (The value in the `Endpoints` is the IP address of Pod.)
4. To see all related things together `kubectl get all | grep mongodb`.
5. Create the config map `kubectl apply -f mongo-configmap.yaml`.
6. setup mongo-express `kubectl apply -f mongo-express.yaml`
7. create the external service (type: Loadbalancer in mongo-express.yaml).
8. 
 