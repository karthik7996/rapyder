#### rapyder
* helm_chartes and jenkins file
* Prometheus
##  Configure Jenkins behind Nginx and Let’s Encrypt SSL
* https://computingforgeeks.com/configure-jenkins-behind-nginx-reverse-proxy-and-lets-encrypt-ssl/
## ARGO CD
* https://blog.fourninecloud.com/installing-argo-cd-using-helm-ed4a0cd0845a
## external-secrets-hashicorp-vault
* https://external-secrets.io/v0.5.7/provider-hashicorp-vault/
## Building Multi-Architecture Container Images
*  docker buildx create --use
* docker buildx build --platform linux/amd64,linux/arm64 --tag 8XXXXXXXXX.dkr.ecr.ap-south-1.amazonaws.com/trace-backend:trace-backend-$CIRCLE_BUILD_NUM --push .
* sed -i "s/tag: trace-backend-[0-9]*/tag: trace-backend-$CIRCLE_BUILD_NUM/g" ./trace-backend/values.yaml
