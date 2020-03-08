# kube8s
1. Create the configmap first used by postgres -- we can use secret instead of configmap which is more secure
   kubectl apply -f postgres-config.yaml 

2. Create the persistent volume and volume claim needed by the postgres database
   kubectl apply -f postgres-pvol.yaml
   
3. Deploy the postgres database and its service exposed using nodeport type
   kubectl apply -f postgres-deployment.yaml
   
   Verify postgres is installed properly using below commands
   apt-get install -y postgresql-client #... install psql client
   psql -h localhost -U 'postgresadmin' -p 31990 postgresdb -c '\du' 
   -- for localhost get ip from cluster-info and use it
   -- for port get kubectl get svc postgres and use the nodeport from the result
   
4. Deploy nginx and its service exposed as loadbalance port type
   kubectl apply -f postgres-deployment.yaml

5. To verify connectivity to postgres db from nginx, we can use a custom docker file image for nginx which install the prerequistes.
   We can try using initcontainer or poststart container lifecycle hook for initialization.
   
