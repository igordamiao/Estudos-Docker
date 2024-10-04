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