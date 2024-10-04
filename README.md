### Documentação para Configuração de Aplicação NestJS com Docker
![image](https://github.com/user-attachments/assets/e7299f5d-2c1b-4005-8088-3407cafd3199)

Esta documentação detalha como configurar e executar uma aplicação NestJS dentro de um container Docker, incluindo informações sobre redes, volumes, comandos úteis e práticas de otimização.

**Índice**
1. [Requisitos](#requisitos)
2. [Estrutura do Dockerfile](#estrutura-do-dockerfile)
3. [Configuração do .dockerignore](#configuracao-do-dockerignore)
4. [Clonando a Aplicação com NestJS CLI](#clone-app)
5. [Construção e Execução da Imagem com Network e Volumes](#imagens-e-volumes)
6. [Comandos Úteis para Administração e Monitoramento](#comandos)

## Requisitos
* Docker instalado
* Código da aplicação NestJS com arquivos package.json, yarn.lock e .dockerignore configurado
* Aplicação baseada em um clone da versão mais recente do NestJS CLI

## Estrutura do Dockerfile
> O Dockerfile define o ambiente de execução para a aplicação NestJS.

``` dockerfile
# Imagem base
FROM node:18-slim

# Diretório de trabalho
WORKDIR /usr/src/app

# Copia os arquivos de dependências
COPY package*.json ./

# Instala as dependências
RUN yarn

# Copia o restante do código da aplicação
COPY . .

# Compila a aplicação
RUN yarn run build

# Expõe a porta 3000
EXPOSE 3000

# Comando de inicialização
CMD ["yarn", "run", "start"]
```

## Configuracao do dockerignore
> Para otimizar a imagem, utilize um arquivo .dockerignore para ignorar arquivos desnecessários:

```  plaintext
node_modules
dist
*.log
*.md
.git
.gitignore
Dockerfile
.dockerignore
.env
```

## Clone App
> A aplicação é baseada no NestJS CLI. Para clonar e iniciar a aplicação:
1. Instalar o NestJS CLI:

``` bash
npm install -g @nestjs/cli
```

2. Criar uma Nova Aplicação:
``` bash
npx @nestjs/cli@latest new nome-do-projeto
```
3. Entrar no Diretório do Projeto:

``` bash
cd nome-do-projeto
```
5. Instalar Dependências e Testar Localmente:

```bash
yarn install
yarn start
```

## Imagens e Volumes
1. Criação da Imagem
> No diretório onde está o Dockerfile, construa a imagem:

```bash
docker build -t <nome-da-imagem> .
```
2. Configuração de Rede
Para criar uma rede personalizada, execute:

```bash
docker network create minha_rede
```
3. Configuração de Volumes
Para persistência de dados, crie um volume:

``` bash
docker volume create meu_volume
```
4. Execução do Container
Execute o container mapeando a rede e o volume criados:

```bash

docker run -d \
   --name meu_container \
   --network minha_rede \
   -v meu_volume:/usr/src/app/data \
   -p 3000:3000 \
   <nome-da-imagem>
```
## Comandos
> Gerenciamento de Containers

Parar um Container:
``` bash
docker stop <id-ou-nome-do-container>
```

Iniciar um Container Parado:
```bash
docker start <id-ou-nome-do-container>
```

Reiniciar um Container:
```bash
docker restart <id-ou-nome-do-container>
```

Remover um Container:
``` bash
docker rm <id-ou-nome-do-container>
```

Remover Todos os Containers Parados:
``` bash
docker container prune
```

**Gerenciamento de Imagens**

Listar Todas as Imagens:
``` bash
docker images -a
```

Remover uma Imagem:
``` bash
docker rmi <id-ou-nome-da-imagem>
```

Remover Todas as Imagens Não Utilizadas:
```bash
docker image prune -a
```

**Monitoramento de Containers** 

Verificar Logs de um Container:
```  bash
docker logs <id-ou-nome-do-container>
```

Monitorar Logs em Tempo Real:
``` bash
docker logs -f <id-ou-nome-do-container>
``` 

Verificar Uso de Recursos:
``` bash
docker stats
``` 
**Inspeção e Diagnóstico**

Inspecionar Detalhes de um Container:
``` bash
docker inspect <id-ou-nome-do-container>
```

Inspecionar uma Network:
```  bash
docker network inspect <nome-da-rede>
```

**Gerenciamento de Volumes**

Criar um Novo Volume:
``` bash
docker volume create <nome-do-volume>
``` 

Remover Todos os Volumes Não Utilizados:
``` bash
docker volume prune
```

Inspecionar um Volume:
``` bash
docker volume inspect <nome-do-volume>
```

**Networking**

Conectar um Container a uma Rede:
 ``` bash
docker network connect <nome-da-rede> <id-ou-nome-do-container>
```

Desconectar um Container de uma Rede:
``` bash
docker network disconnect <nome-da-rede> <id-ou-nome-do-container>
```

**Comandos Avançados**

Executar um Comando em um Container Ativo:
```bash
docker exec -it <id-ou-nome-do-container> /bin/bash
```

Exportar o Sistema de Arquivos de um Container:
``` bash
docker export -o <arquivo.tar> <id-ou-nome-do-container>
```
Importar um Sistema de Arquivos:

```bash
docker import <arquivo.tar> <nome-da-nova-imagem>
```

Remover Todos os Containers, Imagens, Redes e Volumes Não Utilizados:
```bash
docker system prune -a --volumes
```
