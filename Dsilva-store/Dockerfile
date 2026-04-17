# FROM node:latest

# # Diretório de trabalho dentro do container
# WORKDIR /app

# # Copia apenas os arquivos de dependências primeiro (cache mais eficiente)
# COPY package*.json ./

# # Instala dependências
# RUN npm install

# # Copia o restante do projeto
# COPY . .

# # Compila o projeto (necessário para produção)
# RUN npm run build

# # Porta padrão do Medusa
# EXPOSE 9000

# # Comando para iniciar o servidor
# CMD ["npm", "run", "start"]

FROM node:23 AS builder

WORKDIR /app/Dsilva-store

COPY . .

RUN rm -rf node_modules

RUN apt-get update && apt-get install -y python3 python3-pip python-is-python3

RUN corepack enable

RUN corepack prepare yarn@1.22.22 --activate

RUN yarn install --loglevel=error

RUN yarn build


FROM node:23

WORKDIR /app/Dsilva-store

RUN apt-get update && apt-get install -y python3 python3-pip python-is-python3

RUN corepack enable

RUN mkdir .medusa

COPY --from=builder /app/Dsilva-store/.medusa ./.medusa

WORKDIR /app/Dsilva-store/.medusa/server

RUN yarn install --production

CMD ["yarn", "start"]
