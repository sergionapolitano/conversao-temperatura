# conversao-temperatura

## _Exemplo de execução de um app em cluster kubernetes_
####  _Neste projeto teste o k3d foi utilizado para gerenciar clusters da distro k3s_

## Pré-requisitos:

- Docker devidamente instalado no host onde pretende rodar o projeto (o docker será usado para criar imagens e também para executar o k3s).
- k3d devidamente instalado e configurado (veja mais detalhes e instruções de como fazer [aqui](https://github.com/k3d-io/k3d) e [aqui](https://k3d.io/v5.4.1/#installation)
- kubectl devidamente instalado e configurado (veja instruções de como fazer [aqui](https://kubernetes.io/docs/tasks/tools/).
- Clonar via git ou baixar e descompactar em um diretório do host onde foi instalado o k3d


## Exemplo de como executar o projeto:

Navegar até o diretório do projeto e executar os comandos a seguir para gerar e tagear a imagem que será utilizada no exemplo:
```sh
docker build -t sua-conta-no-dockerhub/conversao-temperatura:v1 .
docker tag sua-conta-no-dockerhub/conversao-temperatura:v1 sua-conta-no-dockerhub/conversao-temperatura:latest
docker push sua-conta-no-dockerhub/conversao-temperatura:latest
docker push sua-conta-no-dockerhub/conversao-temperatura:v1
```

Se tudo correr bem com a geração da imagem docker, vc poderá testar a execução de containers baseados na nova imagem com o comando:
```sh
docker run --name teste-ct -d -p 8081:8080 sua-conta-no-dockerhub/conversao-temperatura:v1 
```
E a aplicação poderá ser acessada pelo ip do host docker via browser (por exemplo http://ip-do-host:8081/)

Tudo funcionando, vamos remover o container que foi criado para testes:
```sh
docker container rm -f teste-ct
```



## _Subindo o cluster Kubernetes utilizando as ferramentas k3d e kubectl_
_Iniciando cluster com 3 servers e 3 agents, onde o loadbalancer vai "ouvir" na porta 8080 do ip da rede local._
```sh
k3d cluster create teste --servers 3 --agents 3 -p "8080:30000@loadbalancer"
```

_Realizando deploy da aplicação web no cluster. Ajuste o arquivo para a quantidade de pods desejada_
```sh
kubectl apply -f deployment.yaml
```

Feito isso, basta esperar os pods subirem e conferir no navegador via browser (por exemplo http://ip-do-host:8080/)

Para finalizar a aplicação, basta parar o deploy e o cluster com os comandos
```sh
kubectl delete deployments web
k3d cluster stop teste
```

Thats all folks
