// deployment 생성
kubectl create deployment secloudit-deployment --image=nginx --port 80 --replicas 3

// service 생성
kubectl expose deployment secloudit-deployment --port 80 --target-port 80 --name secloudit-service --type=ClusterIP

// ingress 생성
kubectl create ingress secloudit-ingress --class=nginx --rule="secloudit.ingress.com/=secloudit-service:80"