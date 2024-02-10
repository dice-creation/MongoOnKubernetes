
Set up: kubectl get all 
    minikube start
    kubectl config get-contexts
    kubectl config use-context <context-name>

Step 1: setup mongodb secret for login to mongdb
    use echo -n "username" | base64
    use echo -n "password" | base64
    kubectl apply -f mongodb-secret.yaml
    kubectl get secret

Step 2: setup mongodb deployment for accessing mongdb
    configure with password from mongodb-secret.yaml
    kubectl apply -f mongodb-deployment.yaml
    kubectl get pod --watch
    kubectl describe pod <pod-name> # for analysis

Step 3: setup mongodb service with deployment
    kubetl apply -f mongodb-deployment.yaml
    kubectl get service
    kubectl describe service <service-name>
    kubectl get pod -o wide

Step 4: setup ConfigMap DB Url for centralized external configuration first then deployment
    setup mongodb express for deployment which connects the express UI to deployment
    setup MongoExpress external service
    kubectl apply -f mongodb-configmap.yaml

Step 5: Running mongoDB Url
    kubectl get service
    minikube service mongo-express-service

# username: admin
# password: pass



To curl request:
curl -u admin:pass -X GET http://127.0.0.1:51568/db/dog_dump/dog_data > curl.txt


curl -u admin:pass -X GET http://127.0.0.1:51568/db/dog_dump/dog_data/65c59cd11f1c012e8e7a8f16
 -H "Content-Type: application/ejson" -H "Accept: application/json" --data-raw '{
    "database":"dog_dump",
    "collection":"dog_data",
}'