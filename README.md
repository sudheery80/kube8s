# kube8s
<<<<<.....Create a Service and expose it......>>>>
1. Create configmap first used by postgres db -- we can use secret instead of configmap which is more secure
   
   kubectl apply -f postgres-config.yaml 

2. Create persistent volume and volume claim needed by the postgres database
   
   kubectl apply -f postgres-pvol.yaml
   
3. Deploy postgres database and its service exposed using nodeport type
   
   kubectl apply -f postgres-deployment.yaml
   
   Verify postgres is installed properly using below commands
   
   apt-get install -y postgresql-client #... install psql client
   
   psql -h localhost -U 'postgresadmin' -p 31990 postgresdb -c '\du' 
   -- for localhost get ip from cluster-info and use it
   -- for port get kubectl get svc postgres and use the nodeport from the result
   
4. Deploy nginx and its service exposed as loadbalancer port type
   
   kubectl apply -f postgres-deployment.yaml
   
5. We can get the svc details and curl the ip to check is nginx is working
  
   kubectl describe svc nginx-service

<<<<<.....Create a Route in Openshift kubernetes platform......>>>>
1. Create nginx deploymentconfig using nginx-proxy.yaml
   
   oc apply -f nginx-proxy.yaml

2. Expose the deploymentconfig as a service
   
   oc create service clusterip nginx-proxy --tcp=80:80

3. Create routes using nginx-proxy-route.yaml
   
   oc apply -f nginx-proxy-route.yaml
   
   we can check the routes are created and check the connectivity using curl with host
   ex:oc get routes
NAME                HOST/PORT                                           PATH      SERVICES      PORT      TERMINATION   WILDCARD
nginx-proxy-edge    nginx-proxy-edge-myproject.192.168.99.101.nip.io              nginx-proxy   80-80     edge          None
nginx-proxy-plain   nginx-proxy-plain-myproject.192.168.99.101.nip.io             nginx-proxy   80-80                   None



