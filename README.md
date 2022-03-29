# conversao-temperatura

## _Exemplo de execução do projeto conversao-temperatura em um container docker_

## Pré-requisitos:

- Docker pré-instalado no host onde pretende rodar o projeto
- Clonar via git ou baixar e descompactar em um diretório do host docker


## Modo de instalar e executar:

Navegar até o diretório do projeto e executar o comando seguinte para gerar a imagem:
```sh
docker build -t conversao-temperatura:v1 .
```

Se tudo correr bem com a geração da imagem docker, vc poderá executar containers baseados na nova imagem com o comando:
```sh
docker run -d -p 8080:8080 conversao-temperatura:v1 
```
A partir de então, a aplicação poderá ser acessada pelo ip do host docker via browser (por exemplo http://ip-da-maquina-docker:8080/)

Thats all folks
