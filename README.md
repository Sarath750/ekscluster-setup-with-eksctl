# ekscluster-setup-with-eksctl



Install kubectl
---------------
curl -o kubectl https://s3.us-west-2.amazonaws.com/amazon-eks/1.22.6/2022-03-09/bin/linux/amd64/kubectl
chmod +x ./kubectl
cp kubectl /usr/bin
kubectl version --short --client

Install eksctl
--------------
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/bin
eksctl version


For Master/Cluster creation
---------------------------
eksctl create cluster --name=eksdemo \
                  --region=us-east-1 \
                  --zones=us-east-1a,us-east-1b,us-east-1c \
                  --without-nodegroup 

For Node-group creation
-----------------------
eksctl create nodegroup --cluster=eksdemo \
                   --region=us-east-1 \
                   --name=eksdemo-ng-public \
                   --node-type=t2.medium \
                   --nodes=2 \
                   --nodes-min=2 \
                   --nodes-max=4 \
                   --node-volume-size=10 \
                   --ssh-access \
                   --ssh-public-key=8ambatch \
                   --managed \
                   --asg-access \
                   --external-dns-access \
                   --full-ecr-access \
                   --appmesh-access \
                   --alb-ingress-access	

Add Iam-Oidc-Providers
----------------------
eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster eksdemo \
--approve
