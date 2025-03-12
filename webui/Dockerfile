FROM node:18

# Cria diretório e define como padrão
WORKDIR /app

# Instala dependências do projeto (substitua pela sua WebUI)
COPY . .
RUN npm install

# Expõe a porta do WebUI
EXPOSE 3000

CMD ["npm", "start"]
