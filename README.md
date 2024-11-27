# FECP4-1022-Lab1
Submission for FECP4-1022-Lab 1: Deploying environments in Kubernetes

Steps:

1. Create a namespace for production
     kubectl create namespace production

2. Create a configmap for production
    kubectl create configmap app-config \
   --from-literal=APP_ENV=production \
   --namespace=production

3. Create a secret for production
    kubectl create secret generic app-secret \
   --from-literal=DB_PASSWORD=prodpassword123 \
   --namespace=production

4. Create a YAML file for the deployment to be used in production using the template specified (app-deployment-template.yaml) and change number of replicas to 3
   sed 's/PLACEHOLDER_NAMESPACE/development/' app-deployment-template.yaml > dev-deployment.yaml
   -e 's/replicas: .*$/replicas: 3/' app-deployment-template.yaml > production-deployment.yaml

5. Apply the deployment using the templated that was edited
   kubectl apply -f production-deployment.yaml

7. Expose the production environment
   kubectl expose deployment nginx-app \
   --type=NodePort \
   --name=nginx-service \
   --port=80 \
   --target-port=80 \
   --namespace=production

8. Verify the deployment by executing pod and checking env variables
   
   kubectl get pods -n production
   kubectl exec -it nginx-app-65968468b6-x64tw -n production -- /bin/bash
           //(one pod's name is nginx-app-65968468b6-x64tw)
   echo $APP_ENV
   echo $DB_PASSWORD
