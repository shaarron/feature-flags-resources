### Prerequisits 
create namespaces:

kubectl create namespace kps
kubectl create namespace efk
kubectl create namespace mongodb

install crds  & operators

create secrets

kubectl create secret docker-registry ecr-creds \           
  --docker-server=869432184848.dkr.ecr.ap-south-1.amazonaws.com \
  --docker-username=AWS \
  --docker-password="$(aws ecr get-login-password --region ap-south-1)" \
  --namespace default

secret/ecr-creds created


kubectl create secret generic mongo-secret -n default \     
  --from-literal=MONGO_INITDB_ROOT_USERNAME='root1' \            
  --from-literal=MONGO_INITDB_ROOT_PASSWORD='cm9vdDE=' \
  --from-literal=APP_USER_PASSWORD='YXBw'                                
  
kubectl create secret generic mongo-secret -n mongodb \
  --from-literal=MONGO_INITDB_ROOT_USERNAME='root1' \
  --from-literal=MONGO_INITDB_ROOT_PASSWORD='cm9vdDE=' \
  --from-literal=APP_USER_PASSWORD='YXBw'
