# Fase 02 - Artefatos de entrega

## Deploy Kubernetes

Para o deploys das aplicações criamos 2 deploys.

1- Aplicação - [API Self Order](https://github.com/colantuomo/order-self-service-api)

2- Banco de dados da aplicação - Postgres

### Arquivos yaml - Namespace

Para isolar o projeto de outras Namespaces criamos a namespace `order-self-service` e aplicamos nas configurações para que obedecessem essa configuração. 

Para rodar a criação do namespace rodar o comando abaixo:

```bash
kubectl apply -f namespace.yaml
```

### Arquivos yaml - Aplicação

1- app-deployment: Arquivo principal do Deploy do projeto.

Configuramos o Deploy da aplicação para gerar 2 réplicas do projeto.

A imagem está no DockerHub sob a conta do [willkazahaya](https://hub.docker.com/repository/docker/willkazahaya/order-self-service/general).

O deploy da aplicação utiliza o arquivo de `ConfigMap` (app-configmap-env) para definir a conexão com o banco de dados, a API do mercado pago e a porta que o projeto irá subir. Assim como tambem conta com o arquivo `Secret` (app-secret) que contem o token para a integração do MercadoPago encriptografado em Base64.

2- app-service: Para disponibilizar o projeto utilizamos um Service do tipo ClusterIP que disponibiliza a porta 8080 para uso, vinculando à porta 8080

### Arquivos yaml - Banco de Dados

1- database-persistent-volume: Começamos gerando o Volume de 2GB para o Banco para que os dados fossem persistidos

2- database-persistent-volume-claim: Vinculamos o volume para poder utilizar no Deploy do banco de dados

3- database-deployment: No arquivo do deployment definimos uma unica réplica, disponibilizando a porta 5432


## Database
### Namespace
kubectl apply -f namespace.yaml

### Volumes
kubectl apply -f database-persistent-volume.yaml
kubectl apply -f database-persistent-volume-claim.yaml

### Deployment
kubectl apply -f database-deployment.yaml

### Service
kubectl apply -f database-service.yaml

# Order dos comandos - Exclusão
kubectl delete deploy database
kubectl delete pvc database-persistent-volume-claim
kubectl delete pv database-persistent-volume

# Disponibilidade local
- Acesso ao Banco de Dados:
    - Porta localhost:5432