## How to Deploy a Sample Java based web application and tomcat as web-server


## Pre-requisites
1. War File. Repo already has a sample war file called example.war. If you want another one, you can download one from the below site,
https://www.middlewareinventory.com/blog/sample-web-application-war-file-download/ 

2. Docker installed on your local machine.
3. Docker Hub account to push the Docker image that you are going to create.
4. Kubernetes cluster and Helm 3.xx installed.
5. kubectl utility


## Step 1:  Build your custom Docker image.

 A dockerfile already exists in the directory. You will need to rename the war file that you are going to use. Since my war file name is example.war file, I have mentioned the same in the second instruction of the Dockerfile.

 Build the Docker image, replacing the USERNAME placeholder in the command below with your Docker Hub username:

 `docker build -t USERNAME/java-app .`

 ## Note:
  Username will be your dockerhub account username. You can name the app name to whatever you wish to. 

Once you build, test your app by deploying it on your local environment with the below command,

`docker run USERNAME/java-app`

Your application should get deployed. You can check by watching out for the following line in your output,

`INFO [main] org.apache.catalina.startup.HostConfig.deployWAR Deploying web application archive [/bitnami/tomcat/data/example.war]`

Once you see the above, your app image works fine and you are now good to push it to dockerhub.

## Step 2: Publish the Docker image

 The reason why we are pushing to dockerhub is we will be pulling it from dockerhub later-on to deploy on our Kubernetes cluster.

 So first login into your account from cli with the below,

 `docker login`

 Push the image,

 `docker push USERNAME/java-app`

 ## Note: 
 Username will be your dockerhub account username. You can name the app name to whatever you wish to.

 Once it completes successfully it means its on the internet and anyone can download your baby ;)

 ## Step 3: Deploy the application on Kubernetes

 Once you are ready on your kubernetes cluster and also helm installed(As mentioned in Pre-requisites), Go ahead and execute the below helm commands to add the repo and install the Bitnami's Apache Tomcat Helm chart. The reason why we are using Bitnami is it has in-build support for custom docker images that we create for our apps.

`helm repo add bitnami https://charts.bitnami.com/bitnami`
`helm install my-tomcat-app --set image.repository=USERNAME/java-app --set image.tag=latest --set image.pullPolicy=Always bitnami/tomcat`

## Note:
 Username will be your dockerhub account username. Name of the app will be the same name that you gave for the image when you pushed your app image to dockerhub in the previous steps.



This will create a pod within the cluster for the Tomcat service


Once the deploy is over, check the running pods with `kubectl get pods`

## To obtain the application URL, run the commands shown in the "Notes" section and browse to the WAR application at the resulting service IP address, such as http://IP-ADDRESS/example. 

BOOM, You've now successfully deployed your Java/Tomcat application on Kubernetes. 











