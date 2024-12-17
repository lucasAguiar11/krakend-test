# Build stage
FROM node:18-alpine as builder

# Create app directory
WORKDIR /app

# Copia apenas os arquivos necessários para instalação de dependências
COPY package*.json ./

# Instala dependências e constrói o projeto
RUN npm install
COPY . .
RUN npm run build

# Estágio final
FROM node:18-alpine

WORKDIR /app

RUN apk add --no-cache openssl

# Copia os arquivos necessários do estágio de build
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/dist ./dist

EXPOSE 3000 50051

CMD [ "npm", "run", "start:prod" ]