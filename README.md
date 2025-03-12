# Ollama AI com WebUI - Docker Setup

Este projeto configura um ambiente baseado em Docker para rodar o **Ollama AI** como um servidor de modelos de IA e uma **WebUI** para interação via chat.

## 📌 Requisitos

- Docker e Docker Compose instalados no sistema.
- Conexão com a internet para baixar os modelos e dependências.

## 📦 Estrutura do Projeto

```
.
├── docker-compose.yml
├── ollama/
│   ├── Dockerfile
│   ├── modelos/  # (Opcional: para armazenar modelos baixados)
└── webui/
    ├── Dockerfile
    ├── app/      # (Seu código WebUI)
```

## 🚀 Instalação e Execução

1. **Clone o repositório:**

   ```sh
   git clone https://github.com/seu-repo/ollama-docker.git
   cd ollama-docker
   ```

2. **Suba os containers:**

   ```sh
   docker-compose up -d
   ```

3. **Acesse os serviços:**

   - **Ollama API:** `http://localhost:11434`
   - **WebUI:** `http://localhost:3000`

## 📜 Configuração do Docker

### Dockerfile do **Ollama Server** (`./ollama/Dockerfile`)

```dockerfile
FROM ubuntu:22.04
RUN apt update && apt install -y curl && rm -rf /var/lib/apt/lists/*
RUN curl -fsSL https://ollama.ai/install.sh | bash
RUN ollama pull deepseek
EXPOSE 11434
CMD ["ollama", "serve"]
```

### Dockerfile do **WebUI** (`./webui/Dockerfile`)

```dockerfile
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["npm", "start"]
```

### `docker-compose.yml`

```yaml
version: '3.8'

services:
  ollama:
    build: ./ollama
    ports:
      - "11434:11434"
    restart: always

  webui:
    build: ./webui
    ports:
      - "3000:3000"
    depends_on:
      - ollama
    restart: always
```

## 🛠 Comandos Úteis

### Parar e remover containers

```sh
docker-compose down
```

### Ver logs

```sh
docker-compose logs -f
```

### Acessar o container

```sh
docker exec -it nome_do_container bash
```

## 📝 Contribuição

Sinta-se à vontade para abrir **issues** e **pull requests** para melhorar o projeto! 🚀

## 📄 Licença

GNU General Public License v3.0
