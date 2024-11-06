### You recieved this file from the developers. start by

1. Test the application locally
2. Dockerize it and test again
3. Containerize it and test again
4. Automate it using CICD.

## Test the app locally

1. build the app using // go build - o app .
2. run the app ./app
3. check localhost:8080/courses and see how the site should look like.

### Containerize it

1. Build a Dockerfile - multi-stage dockerfile.
1. first stage will be to build the image using a base image file and then copy only the binary for the second stage. This ensures that only the dependencies that are required are used for the second stage and it is free from vulnerabilities. so the second stage is to..

2. Use the Distroless base image to build and expose the application. This reduces the size of the image and makes your application light weight.

3. run docker build -t <docker-username>/static_webapp:v1

4. run  docker run -p 8080:8080 -it <docker-username>/static-webapp:v1

### Push the image to docker hub repository

1. docker push <docker-username>/static_webapp:v1

### For deploying applications on EKS Cluster

1. Create deployment.yaml

2. Create Service.yaml

3. Create ingress.yaml

4. run eksctl create cluster --name demo-cluster --region us-east-1

5. once done, run kubectl apply -f k8s/manifests/deployment.yaml

6. verify with kubectl get pods

7. run kubectl apply -f k8s/manifests/service.yaml

8. run kubectl apply -f k8s/manifests/ingress.yaml

9. verify with kubectl get ing

10. run kubectl edit svc <servicename> and

11. edit type from ClusterIp to NodePort

12. run kubectl get svc and copy the port where the service is exposed.

13. also run kubectl get nodes -o wide and copy the external IP address

14. create ingress controller - go to the github documentation and copy the link

15. kubectl apply -f <https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.12.0-beta.0/deploy/static/provider/cloud/deploy.yaml> // this will create the ingress controller and configure the load balancer

16. run kubectl get pods -n ingress-nginx

17. run kubectl get ing // see ingress controller watching the ingress resources

18. copy the load balancer name and run ns lookup <loadbalancer_name> and get the IP address

19. go to sudo vim etc/hosts

20. provide the password and map the IP address to static_webapp.loacl

21. go to your browser and type static_webapp.local/home

### Now continue with Helm Chart

1. cd into the helm folder and

2. run helm create static-webapp-chart

3. kubectl get all and delete all resources

4. run helm install static-web-app ./static-webapp-chart.
   
5. Verify by running kubectl get deploy, svc and ing // you will note that all resources have been reinstalled using helm.
   
6. To uninstall, run helm uninstall static-web-app 



### Next is CI-CD.
1. First Stage is Build and Unit Testing
2. Second Stage is Static Code Analysis
3. Third Stage is Building the Docker image
4. Fourth Stage is updating Helm // which will pull a new tag from Value.yaml once there is a commit.

5. Then ArgoCD watches for changes on the Helm Chart and 
6. Updates the manifest to the K8s Cluster.

### Steps;
1. Create a folder called .github
2. Inside the folder, create another folder called workflows and then
3. create a file called ci.yaml and write the syntax as shown on ci.yaml on the folder on the left
   