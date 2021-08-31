
POD_NAME=`kubectl get pods -o jsonpath='{.items[0].metadata.name}'`

kubectl expose deployment frontend --port=80 --target-port=80

FRONTEND_SERVICE_IP=`kubectl get service/frontend -o jsonpath='{.spec.clusterIP}'`

FRONTEND_POD_NAME=`kubectl get pods --no-headers -o custom-columns=":metadata.name"`

kubectl exec -ti $FRONTEND_POD_NAME  -c nginx -- ls -al /var/log/nginx

helm template --name cloudacademy app/ | kubectl apply -f -

helm template --name cloudacademy -f ./app/values.dev.yaml ./app/ | kubectl apply -f -

helm template --name cloudacademy -f ./app/values.prod.yaml ./app/ | kubectl apply -f -







CLOUDACADEMY_APP_IP=`kubectl get service/cloudacademy-app -o jsonpath='{.spec.clusterIP}'`