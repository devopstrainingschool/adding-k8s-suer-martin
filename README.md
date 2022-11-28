# adding-k8s-suer-martin
 
 
## task:
 
###Build user information for martin in the default kubeconfig file: User = martin , client-key = /root/martin.key and client-certificate = /root/martin.crt

###Create a new context called 'developer' in the default kubeconfig file with 'user = martin' and 'cluster = kubernetes'

 
 
 
## kubectl get ns
###    2  kubectl get pods -n development
###   3  openssl req -new -key jbeda.pem -out jbeda-csr.pem -subj "/CN=jbeda/O=developer"
###    4  openssl genrsa -out martin.key 2048
###   5  openssl req -new -key martin.key -out martin.csr -subj "/CN=martin/O=developer"
###    6  
 ###   8  ls /etc/kubernetes/pki/
 ###   9  ls
###   28  cp /etc/kubernetes/pki/ca.crt .
###   29  cp /etc/kubernetes/pki/ca.key .
###   30  openssl x509 -req -in martin.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out martin.crt -days 3540
###   31  ls
###   32  kubectl config get-users
###   33  kubectl config set-credentials jean   --client-certificate=/home/jean/.certs/jean.crt   --client-key=martin.key
###   34  kubectl config set-credentials martin   --client-certificate=martin.crt   --client-key=martin.key
 ###  35  kubectl config get-users
###   36  history
   # New is create in kubeconfig after the above steps
   
   # below steps give autho and authentification
 ```  
   kubectl create role developer-role --verb=* --resource=*
    kubectl describe role developer-role -n development
    ```
```    
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
```
  
  # add user
  https://www.adaltas.com/en/2019/08/07/users-rbac-kubernetes/
  
 # CREATE KUBECONFIG FILE
 default kubeconfig file path: ~/.kube/config
 
 
 apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUMvakNDQWVhZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJeU1URXlOekl6TVRjME1Gb1hEVE15TVRFeU5ESXpNVGMwTUZvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTkpkClFIRGpDVFNHMC92UHcrWHZNOGhhRlBxdlEvYm5vdEZNeWYwVlZXelk4MUZXQ0Z5NmI2dHZ4VHhWaXZZRVBQRXEKSDFQTldzZ2l1QStMOGlRNTUvQzBjM1crSEJy
    server: https://controlplane:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
- context:    
    cluster: kubernetes
    user: martin
  name: developer
current-context: kubernetes-admin@kubernetes
kind: Config
preferences: {}
users:
- name: jean
  user:
    client-certificate: /home/jean/.certs/jean.crt
    client-key: /root/martin.key
- name: kubernetes-admin
  user:
    client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURJVENDQWdtZ0F3SUJBZ0lJWWNaMEVqVDVuN0F3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TWpFeE1qY3lNekUzTkRCYUZ3MHlNekV4TWpjeU16RTNOREphTURReApGekFWQmdOVkJBb1REbk41YzNSbGJUcHRZWE4wWlhKek1Sa3dGd1lEVlFRREV4QnJkV0psY201bGRHVnpMV0ZrCmJXbHVNSUlCSWpBTkJna3Foa2lHOXcwQkFRRUZBQU9DQVE4QU1JSUJDZ
    client-key-data: LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFcEFJQkFBS0NBUUVBNXJKdU9kQVNiZjVuWEFvZkFvR2xNVTRjcEZSVTR0RkRVNjlJMUZqK01ITmd2QVV4ClhNZTN6OUR2V2VGZzlhejBabUFucUI0RkFBRHN2TkpuUGJzTVNRNEVpcmlwQ2U0SHNJcWNrT0kwcWZvdzN
- name: martin
  user:
    client-certificate: /root/martin.crt
    client-key: /root/martin.key
 
 
