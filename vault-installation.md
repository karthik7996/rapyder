# Vault setup
  
## Consul install
  * sudo su -
  * sudo apt install unzip
  * sudo apt-update install
  * wget https://releases.hashicorp.com/consul/1.13.1/consul_1.13.1_linux_amd64.zip
  * unzip consul_1.13.1_linux_amd64.zip
  * sudo mv consul /usr/bin/
  * consul --version
  * sudo nano /etc/systemd/system/consul.service
```yaml
[Unit]
Description=Consul
Documentation=https://www.consul.io/
[Service]
ExecStart=/usr/bin/consul agent -server -ui -data-dir=/temp/consul -bootstrap-expect=1 -node=vault -bind=0.0.0.0 -config-dir=/etc/consul.d/
ExecReload=/bin/kill -HUP $MAINPID
LimitNOFILE=65536
[Install]
WantedBy=multi-user.target
```
  

* sudo mkdir /etc/consul.d
* sudo nano /etc/consul.d/ui.json
```json
{
 "addresses": {
   "http": "0.0.0.0"
 }
}
```
* sudo systemctl daemon-reload
* sudo systemctl start consul.service
* sudo systemctl status consul.service
* sudo systemctl enable consul.service


# Install vault

* wget https://releases.hashicorp.com/vault/1.10.1/vault_1.10.1_linux_amd64.zip
* unzip vault_1.10.1_linux_amd64.zip
* mv vault /usr/bin/
* vault --version
* sudo mkdir /etc/vault
* sudo nano /etc/vault/config.hcl
```json
storage "consul" {
address = "127.0.0.1:8500"
path = "vault/"
}
listener "tcp" {
address = "0.0.0.0:8200"
tls_disable = 1
}
ui = true
```
* sudo nano /etc/systemd/system/vault.service
```json
[Unit]
Description=Vault
Documentation=https://www.vault.io/
[Service]
ExecStart=/usr/bin/vault server -config=/etc/vault/config.hcl
ExecReload=/bin/kill -HUP $MAINPID
LimitNOFILE=65536
[Install]
WantedBy=multi-user.target
```
sudo systemctl daemon-reload
sudo systemctl start vault.service
sudo systemctl status vault.service
sudo systemctl enable vault.service

export VAULT_ADDR='http://13.234.231.137:8200/'
export VAULT_TOKEN="hvs.uWSAhWqb9AjTjIHtBGkQcwD3"
vault operator init


Unseal Key 1: 
Unseal Key 2: 
Unseal Key 3: 
Unseal Key 4: 
Unseal Key 5: 

Initial Root Token: 







vault operator unseal 
vault login

Secrets


vault secrets disable kv
vault secrets enable kv

vault kv put kv/secret Hello=Preethi
vault kv get kv/secret/
vault kv get -format=json kv/secret | jq

transit

 vault secrets enable transit
 vault write -f transit/keys/test-key
 vault write transit/encrypt/test-key plaintext=$(echo "test123" | base64)
