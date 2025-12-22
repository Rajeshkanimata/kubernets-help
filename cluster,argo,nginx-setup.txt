-----------------------CLUSTER CREATION------------------------------------------
create aclutser with basic IAM role for cluster in custom configuration
disable EKS auto mode
and select network settings like VPC, Subnets,SG Group etc.
and check all the realted variables and settings and create  

usally take 5 mins

-------------------- NODEGROUP-----------------------------------------------------

open cluster ----->> compute ------>> node group
create a node group ----->> name ---->> instamce tyoe usally t3.small forr free tier

usually takes 10 mins

---------------------CLIENT SERVER KUBECTL SETUP---------------------------------------------------
create a ec2 instance 

sudo su -

run theses command to install kubectl 

curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl

chmod +x ./kubectl                                                                                                                                     #Make the kubectl binary executable

sudo mv ./kubectl /usr/local/bin/kubectl                                                                                                               #sudo mv ./kubectl /usr/local/bin/kubectl

KUBE CONGIG

aws eks update-kubeconfig --region <region> --name <cluster-name>         # change region and name

ex.  aws eks update-kubeconfig --region us-east-1 --name rajesh

---------------------------DOCKER IMAGE ----------------------------------------------------------

connect to instance 

yum install docker -y
systemctl start docker
git clone https://github.com/Rajeshkanimata/kubernetes-ingress-deployment.git
cd kubernetes-ingress-deployment/
docker build -t main .
docker build -t aws .
docker build -t azure .
docker build -t gcp .

now push every image into ECR by creating a repo in ECR

---------------------------AGRO UI ------------------------------------------------------

we can check in this link for more info------   https://medium.com/@veerababu.narni232/a-complete-overview-of-argocd-with-a-practical-example-f4a9a8488cf9

kubectl create namespace argocd                                                                                                                         # create a namespace for agrocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml                                             # install argocd agent in client
kubectl get all -n argocd                                                                                                                               #get details of all
kubectl edit svc argocd-server -n argocd                                                                                                                # edit your required service like LB or NodePort
kubectl get secret argocd-initial-admin-secret -n argocd -o yaml                                                                                        # to get secret and next decode and login
and access through lb or node port through  extrernal IP:port 
be sure to enable sg group and any of the worker node to access if using nodeport
-----------------------------NGINX INGRESS CONTROLLER -------------------------------------

install NGNIX ingress controller 
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.11.3/deploy/static/provider/cloud/deploy.yaml
kubectl get pods -n ingress-nginx                                                                                                                       #check status of Nginx controller


 
