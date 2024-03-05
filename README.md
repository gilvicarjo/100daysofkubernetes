# 100daysofkubernetes

## Day 1 - https://kodekloud.com/topic/lab-kubernetes-challenge-1

### How to give access for a user/developer in a Kubernetes Cluster

Step 1 - First, set a CA for the Kubernetes Cluster

The main objective of this step is to have a ca.key and a ca.crt to certify users to access into the kubernetes cluster

Please, explore here: https://kubernetes.io/docs/tasks/administer-cluster/certificates/

Step 2 - Generate client-certificate .crt and client-key .key certificates

1. (User) Generate a private key (.key) for the client:
```
openssl genrsa -out **client-key.key** 2048
```
2. (User) Generate a Certificate Signing Request (CSR) for the client:

```
openssl req -new -key client-key.key -out **client.csr**
```
3. (Admin) Generate the client certificate (.crt) using the CSR and a Certificate Authority (CA):
```
openssl x509 -req -in client.csr -CA **ca.crt** -CAkey **ca.key** -CAcreateserial -out client.crt -days 365
```

Step 3 - Setup credentials in kubeconfig

For example:

#### Build user information for martin in the default kubeconfig file: User = martin , client-key = /root/martin.key and client-certificate = /root/martin.crt (Ensure don't embed within the kubeconfig file)

```
kubectl config set-credentials martin --client-certificate ./martin.crt --client-key ./martin.key
```

#### Create a new context called 'developer' in the default kubeconfig file with 'user = martin' and 'cluster = kubernetes'
```
kubectl config set-context developer --cluster kubernetes --user martin
```
