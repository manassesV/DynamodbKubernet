# DynamoDB Local no Kubernetes

Este projeto configura e executa o **DynamoDB Local** dentro de um cluster Kubernetes para desenvolvimento e testes locais.

## Pré-requisitos

- Kubernetes cluster (minikube, kind, k3s, etc)
- `kubectl` instalado e configurado
- AWS CLI instalado (opcional para comandos AWS)

## Passos para rodar

1. Aplicar o manifesto do DynamoDB Local:

```bash
kubectl apply -f dynamodb-local.yaml
Verificar o status do pod:

bash

kubectl get pods
Fazer port-forward para acessar o DynamoDB Local localmente:

bash

kubectl port-forward pod/dynamodb-local-xxxxx 8000:8000
Substitua dynamodb-local-xxxxx pelo nome do seu pod.

Testar se o DynamoDB está respondendo:

bash

curl -X POST http://localhost:8000 \
  -H "X-Amz-Target: DynamoDB_20120810.ListTables" \
  -H "Content-Type: application/x-amz-json-1.0" \
  -d "{}"
Deve retornar algo assim:

json
Copiar
Editar
{"TableNames":[]}
Usando AWS CLI com DynamoDB Local
Configure o AWS CLI para usar credenciais falsas e região:

bash

aws configure
Preencha com:

pgsql

AWS Access Key ID [None]: fakeMyKeyId
AWS Secret Access Key [None]: fakeSecretAccessKey
Default region name [None]: us-east-1
Default output format [None]: json
Para listar tabelas:

bash
Copiar
Editar
aws dynamodb list-tables --endpoint-url http://localhost:8000
Usando com C#
Exemplo básico para conectar no DynamoDB Local:

csharp
Copiar
Editar
var config = new AmazonDynamoDBConfig
{
    ServiceURL = "http://localhost:8000"
};
var client = new AmazonDynamoDBClient(config);
Observações
DynamoDB Local é somente para desenvolvimento/testes. Não use em produção.

Sempre exponha a porta 8000 para acesso local via port-forward ou Service NodePort.

DynamoDB Local on Kubernetes (English)
This project configures and runs DynamoDB Local inside a Kubernetes cluster for local development and testing.

Prerequisites
Kubernetes cluster (minikube, kind, k3s, etc)

kubectl installed and configured

AWS CLI installed (optional for AWS commands)

Steps to run
Apply the DynamoDB Local manifest:

bash
Copiar
Editar
kubectl apply -f dynamodb-local.yaml
Check pod status:

bash
Copiar
Editar
kubectl get pods
Port-forward to access DynamoDB Local locally:

bash
Copiar
Editar
kubectl port-forward pod/dynamodb-local-xxxxx 8000:8000
Replace dynamodb-local-xxxxx with your pod name.

Test if DynamoDB is responding:

bash
Copiar
Editar
curl -X POST http://localhost:8000 \
  -H "X-Amz-Target: DynamoDB_20120810.ListTables" \
  -H "Content-Type: application/x-amz-json-1.0" \
  -d "{}"
Expected response:

json
Copiar
Editar
{"TableNames":[]}
Using AWS CLI with DynamoDB Local
Configure AWS CLI with fake credentials and region:

bash

aws configure
Fill in:


AWS Access Key ID [None]: fakeMyKeyId
AWS Secret Access Key [None]: fakeSecretAccessKey
Default region name [None]: us-east-1
Default output format [None]: json
List tables:

bash

aws dynamodb list-tables --endpoint-url http://localhost:8000
Using with C#
Basic example to connect to DynamoDB Local:

csharp

var config = new AmazonDynamoDBConfig
{
    ServiceURL = "http://localhost:8000"
};
var client = new AmazonDynamoDBClient(config);
Notes
DynamoDB Local is for development/testing only. Do not use in production.

Always expose port 8000 for local access via port-forward or NodePort Service.
