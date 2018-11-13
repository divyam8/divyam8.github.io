---
title: Using Telepresence
description: telepresence is a tool to ease the process of developing and debugging services locally, while proxying the service to a remote Kubernetes cluster. Using telepresence allows you to use custom tools, such as a debugger and IDE, for a local service and provides the service full access to ConfigMap, secrets, and the services running on the remote cluster.

Files to be download/install:
1.Install telepresence locally
curl -s https://packagecloud.io/install/repositories/datawireio/telepresence/script.deb.sh | sudo bash
sudo apt install --no-install-recommends telepresence
Ref=>https://www.telepresence.io/reference/install

2.Install kubectl
sudo snap install kubectl --classic
Ref=>https://kubernetes.io/docs/tasks/tools/install-kubectl/

3.Download heptio-authenticator-aws_0.3.0_linux_amd64
Ref=>https://github.com/kubernetes-sigs/aws-iam-authenticator/releases
 
Steps:
1. Login with aws-google
   aws-google-auth -d 43200 -p default

2. Set profile
   export AWS_PROFILE=dev-admin

3. Move the downloaded file(heptio-authenticator-aws_0.3.0_linux_amd64) to /usr/local/bin/
 
4. Move config file local folder.  
   cp /k8/kubeconfig /localhost/.kube/config

6. Run 
   telepresence --swap-deployment merchantintegration-nc-320 --method vpn-tcp --expose 5002:80 --also-proxy master.commerce.dev.niki.ai --env-json mi.env.json (pwd should be your service folder)

   merchantintegration-nc-320: This is the service I want to debug/run/build locally. To find this run k get deployment(in dev.niki.ai console)
   --env-json mi.env.json: This is the file created in your service directory when we run this command.


  To configure in Intellij IDE:
  1. Go to Setting->Plugins->Browse Repositories and search EnvFile and install it.
  
  2. Go to Run->Debug Configurations
     Select EnvFile tab and select the .env.json file from the same service directory.    
  
  3. Try to run/build/debug your code locally.
Reference:
https://kubernetes.io/docs/tasks/debug-application-cluster/local-debugging/
https://cloud.google.com/community/tutorials/developing-services-with-k8s
weight: 1
draft: false
---
