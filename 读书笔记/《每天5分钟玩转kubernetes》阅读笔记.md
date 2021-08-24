本读书笔记总结自《每天5分钟玩转kubernetes》
# 1.先把k8s跑起来
本书未读完...

# 附录：PaaS平台使用到的helm、k8s命令
```shell
kubectl create -f test-host-pod.yaml
kubectl delete -f test-host-pod.yaml
kubectl exec -it task-4029-g0-test-server-middleware-test-1-0 cat /etc/hosts
kubectl logs -f task-4029-g0-test-server-middleware-test-1-0
kubectl get pods -o wide | grep host
kubectl get svc
kubectl get pod
kubectl get cm
kubectl get all
kubectl get po
kubectl get nodes
kubectl delete pvc data-mysql-ha-mysql-ha-0 data-mysql-ha-mysql-ha-1 data-mysql-ha-mysql-ha-2

helm install --name mysql-ha mysql-ha/
helm delete --purge mysql-ha
helm status mysql-ha
helm list
```