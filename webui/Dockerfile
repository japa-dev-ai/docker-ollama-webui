# Usa uma imagem base Node.js
FROM node:18

# Define o diretório de trabalho
WORKDIR /app

# Copia os arquivos do projeto
COPY . .

# Instala as dependências
RUN npm install

# Expõe a porta padrão do OpenWebUI
EXPOSE 3000

# Comando para iniciar o OpenWebUI
CMD ["npm", "start"]