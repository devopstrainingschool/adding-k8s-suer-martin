# adding-k8s-suer-martin
kubectl get ns
    2  kubectl get pods -n development
    3  openssl req -new -key jbeda.pem -out jbeda-csr.pem -subj "/CN=jbeda/O=developer"
    4  openssl genrsa -out martin.key 2048
    5  openssl req -new -key martin.key -out martin.csr -subj "/CN=martin/O=developer"
    6  
    8  ls /etc/kubernetes/pki/
    9  ls
   28  cp /etc/kubernetes/pki/ca.crt .
   29  cp /etc/kubernetes/pki/ca.key .
   30  openssl x509 -req -in martin.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out martin.crt -days 3540
   31  ls
   32  kubectl config get-users
   33  kubectl config set-credentials jean   --client-certificate=/home/jean/.certs/jean.crt   --client-key=martin.key
   34  kubectl config set-credentials martin   --client-certificate=martin.crt   --client-key=martin.key
   35  kubectl config get-users
   36  history
   
   
   kubectl create role developer-role --verb=* --resource=*
    kubectl describe role developer-role -n development
    
    
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: role-grantor-binding
  namespace: user-1-namespace
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: role-grantor
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: user-1
  
  
  
 
 
 
 
