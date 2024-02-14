## Installing Argo CD using helm

#### Argo CD is a powerful continuous delivery tool that helps developers automate the deployment process of their applications

#### Before we begin, make sure you have the following prerequisites:
* A Kubernetes cluster (can be local or remote)
* Helm installed on your local machine
* kubectl configured to access your cluster
## Step 1: Add the Argo CD Helm repository
#### helm repo add argo https://argoproj.github.io/argo-helm
* Step 2: Install Argo CD using Helm
## helm install argocd argo/argo-cd
* This will install Argo CD in the default namespace. If you want to install it in a different namespace, you can use the --namespace flag.
* Access the Argo CD dashboard
* To access the Argo CD dashboard, we need to get the URL of the dashboard. To do this,
* Create the ingress for the argo cd service and use the domain name to access it.
* Configuring the a Bitbucket repository
* To deploy applications using Argo CD, you need to set up a Git repository that contains your application source code and deployment manifests. You can use any Git hosting service, such as  Bitbucket, for this purpose.
* Navigate to the settings and click on connect repo, provide the requried details like name main fist file ssh key etc.






* One you added the ssh and it will successful connect to Bit bucket.


#### Creating an application in Argo CD
 # you can now create your first application in Argo CD. To do this, follow these steps:

* In the Argo CD web UI, click on the + New App button.
* In the Repository URL field, enter the URL of your Git repository.
* In the Revision field, enter the branch  of the revision you want to deploy.
* In the Destination section, select the namespace and cluster where you want to deploy the application.
* In the Sync Options section, you can specify additional options for the application, such as the resource filter and sync policy
* Click on the Create button to create the application.
## Once the application is created, Argo CD will automatically sync the application to the destination namespace and deploy it. You can view the status of the deployment and any errors in the Overview tab of the application page.


#### Configuring the k8s to HashiCorp Vault
* Step 1: Add the vault Helm repository
First, we need to add the vault  Helm repository to our local machine:
### helm repo add external-secrets https://charts.external-secrets.io
* Step 2: Install vault using Helm
* Create external secret using below command with required name space.
### helm install external-secrets external-secrets/external-secrets -n external-secrets --create-namespace --set installCRDs=true

### Step 3: First, create a SecretStore with a vault backend
 * Add the vault url with token to connect k8s with vault, create the vault secret stor in all the namespace required
```yaml
apiVersion: external-secrets.io/v1beta1
kind: SecretStore
metadata:
  name: vault-backend
spec:
  provider:
    vault:
      server: "http://XXXXXXXXXX:8200/"
      path: "kv"
      version: "v1"
      auth:
        # points to a secret that contains a vault token
        # https://www.vaultproject.io/docs/auth/token
        tokenSecretRef:
          name: "vault-token"
          key: "token"
---
apiVersion: v1
kind: Secret
metadata:
  name: vault-token
data:
  token: aHZzLkXXXXXXXXXXXXXXXXXXXXXXXX
'''
### NOTE: In case of a ClusterSecretStore, Be sure to provide namespace for tokenSecretRef with the namespace of the secret that we just created.
* Then create a simple k/v pair at path secret/dev
* Now create a ExternalSecret that uses the above SecretStore:

