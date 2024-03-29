# 1. Exec into Vault pod 
```
kubectl exec -it -n vault vault-0 -- /bin/sh
```
# 2. Create Secret
```
vault kv put tekton/database/vault-demo username="Z29rdWxjNzgwOnNhbXBsZXBhc3N3ZA=="  
```
# 3. Create Policy for that Secret with 'Read' capability
```
vault policy write vault-demo - <<EOF
path "tekton/data/database/vault-demo" {
   capabilities = ["read"]
}
EOF
```
# 4. Create Role for the policy
```
vault write auth/kubernetes/role/vault-demo \
      bound_service_account_names=vault-demo \
      bound_service_account_namespaces=vault-demo \
      policies=vault-demo \
     
```
# 5. Create SA as we specified in the role
```
kubectl create sa vault-demo
```
# 6. Apply the deployment
```
kubectl apply -f https://raw.githubusercontent.com/Gokul-C/vault-demo/main/vault-deploy.yaml
```
# 7. Validate the secrets inside the pod
```
kubectl exec -it vault-demo-7d7d4bc754-5nqvq -- cat /home/gokul/config.json
```
