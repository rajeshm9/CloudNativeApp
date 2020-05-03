# CloudNativeApp
Sample Cloud App with Continous Integration, Continous Deployment and Continous Monitoring

1 **Build the Tomcat Application with Actuator enabled for monitoring**

`docker buid . -t rajeshm9/webapp`

`docker run -p 8085:9080 rajeshm9/webapp` (Tested it at local for Endpoint and prometheus
url)

`docker push` (Image is in docker hub , pushed from my account)

2. **Deployed the Tomcat Application on Minikube**

`kubectl apply -f deploy.yaml` (Creation of Deployment using image created in step 1 ,configured liveness (port) and readiness (http) probe , mount access logs foldes)
`Kubectl apply -f Service.yaml `(Exposing Deployment as service on port 8080)

`Kubectl apply -f ingress/ingress-resource.yaml` (Creation of ingress resource pointing to tomcat-service listening on port 8080)


3. **Configuration of Prometheous for Monitoring**

 `kubectl create configmap prometheus-cm --from-file prometheus.yml` (Creation of config which contain the prometheus configuration for scraping application data  points)
	`kubectl apply -f  prometheus-deploy.yaml` (Creation of prometheus deployment and exposing service on Node port)
  
4. **Configuration of EFK Stack for Logging Purpose**

`kubectl apply -f elastic.yaml`  (Elasticsearch Deployment on single node and exposing it on Node port http://192.168.1.8:32759/)

`kubectl apply -f kibana.yaml` (Kibana Deployment and exposing it node port http://192.168.1.8:31320/)

`kubectl  apply -f  fluentd-rbac.yaml` (Creation of Cluster Role for accessing the kube-namespace logs)

`kubectl apply -f  fluentd-daemonset.yaml` (Creation on Fluentd Daemonset)


**Pending**
- Jenkins Pipeline Setup for build, docker image Publish and Deployment on K8S Setup
- Sonarqube for Code Quality Check
- Adding of Testing Tools for Continous Testing.
- Security Tools Integration
- Prometheous AlertManager Intergation
- Grafana Integration

  Sample App Code Taken From: https://github.com/callicoder/spring-boot-actuator-demo
  

** Minikube Setp **
