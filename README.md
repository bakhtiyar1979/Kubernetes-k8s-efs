1. So far for stateful applications EBS (Elastic Block Store) used. Limitation of EBS its available 
in single AZ (Availability Zone)
2. In order to overcome above issue we will use EFS (Elastic File Share) which is managed NFS (Network File Share)
3. By using EFS we can share data/volumes between AZs. 
4. AWS console create EFS 
5. enable "amazon-efs-utils" on all worker nodes --> ssh -i ~/Downloads/<<Your Key Pair>> ec2-user@<<>Public-IP> "sudo yum install -y amazon-efs-utils"
6. create namespace --> kubectl create ns efs 
7. Create RBAC (Role Base Access Control)


EFS stack (Elasticsearch, Fluentd and Kibana)
1. Get more info about K8S cluster --> eksctl get cluster --name Orchsky-cluster --region us-east-1
2. Get running pods in all namespaces --> kubectl get pods --all-namespaces
3. Create dedicated namespace for monitoring --> kubectl create namespace dapr-monitoring
4. List all namespaces --> kubectl get ns
5. Helm charts --> Kubernetes package manager
6. Install helm --> https://helm.sh/docs/intro/install/
7. Add the HELM repo Elasticsearch -->  helm repo add elastic https://helm.elastic.co
8. Update helm repo --> helm repo update 
9. Install elasticsearch using helm --> helm install elasticsearch elastic/elasticsearch -n dapr-monitoring --set replicas=1
10. Check runing pods --> kubectl get pods --namespace=dapr-monitoring -l app=elasticsearch-master -w
11. Install Kibana using HELM --> helm install kibana elastic/kibana -n dapr-monitoring
12. kubectl get pods --namespace=dapr-monitoring -l release=kibana -w
13. kubectl get secrets --namespace=dapr-monitoring elasticsearch-master-credentials -ojsonpath='{.data.password}' | base64 -d
14. kubectl get secrets --namespace=dapr-monitoring kibana-kibana-es-token -ojsonpath='{.data.token}' | base64 -d
15. kubectl apply -f fluentd-config-map.yaml
16. kubectl apply -f fluentd-with-rbac.yaml
17. DeamonSet --> Sometimes we need pods running in all the worker nodes. Lets say we spin more worker nodes 
and in this case pods must be deployed on new worker nodes. Daemonset is the best candiadte for such solutions 
Footer
© 2023 GitHub, Inc.
Footer navigation
Terms
Privacy
Security
Status
Docs
Contact GitHub
Pricing
API
Training
Blog
About