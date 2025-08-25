Apply:

kubectl apply -f app.yaml
kubectl -n demo get deploy,po,svc


Test:

kubectl -n demo port-forward svc/web 8080:80
curl http://localhost:8080


Rollout & rollback:

kubectl -n demo set image deploy/web web=nginx:1.25-alpine
kubectl -n demo rollout status deploy/web
kubectl -n demo rollout undo deploy/web


Show probes/resources:

kubectl -n demo describe deploy web