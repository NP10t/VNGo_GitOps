minikube start --network-plugin=cni --cni=flannel

kubectl port-forward svc/vngo-vngo -n default 8080:8080
kubectl port-forward svc/vngo-mysql -n default 3307:3306

cd vngo-chart
helm dependency build
cd ..
helm install vngo ./vngo-chart --namespace default

kubectl exec -it vngo-vngo-f56cd4db4-bv4vl -n default -- env
kubectl logs -l app=vngo-vngo -n default


helm uninstall vngo --namespace default
helm install vngo ./vngo-chart --namespace default

kubectl exec -it vngo-mysql-77c9b5b777-pqz74 -n default -- bash

kubectl get all