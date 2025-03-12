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
   git clone https://github.com/japa-dev-ai/docker-ollama-webui.git
   cd docker-ollama-webui
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
# Usa uma imagem base com suporte a CUDA
FROM nvidia/cuda:12.2.2-base-ubuntu22.04

# Instala dependências
RUN apt-get update && apt-get install -y \
    wget \
    python3 \
    python3-pip \
    git \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Instala o Ollama
RUN curl -fsSL https://ollama.ai/install.sh | bash
RUN pip install ollama

# Define o diretório de trabalho
WORKDIR /app

# Baixa o modelo deepseek-r1:7b
RUN ollama pull deepseek-r1:7b

# Expõe a porta padrão do Ollama
EXPOSE 11434

# Comando para iniciar o Ollama
CMD ["ollama", "serve"]
```

### Dockerfile do **WebUI** (`./webui/Dockerfile`)

```dockerfile
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
```

### `docker-compose.yml`

```yaml
version: '3.8'

services:
  ollama:
    build:
      context: .
      dockerfile: ./ollama/Dockerfile
    container_name: ollama
    restart: unless-stopped
    environment:
      - NVIDIA_VISIBLE_DEVICES=all # Habilita todas as GPUs disponíveis
      - NVIDIA_DRIVER_CAPABILITIES=compute,utility
    volumes:
      - ollama_data:/app/data # Persiste os dados do Ollama
    networks:
      - ollama_network
    deploy:
      resources:
        reservations:
          devices:
            - capabilities: [gpu] # Reserva recursos de GPU

  openwebui:
    build:
      context: .
      dockerfile: ./webui/Dockerfile
    container_name: openwebui
    restart: unless-stopped
    ports:
      - "3000:3000" # Expõe a porta do OpenWebUI
    environment:
      - OLLAMA_API_URL=http://ollama:11434 # Conecta ao Ollama
    depends_on:
      - ollama
    networks:
      - ollama_network

volumes:
  ollama_data:

networks:
  ollama_network:
    driver: bridge
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
