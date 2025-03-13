# Ollama AI com WebUI - Docker Setup

Este projeto configura um ambiente baseado em Docker para rodar o **Ollama AI** como um servidor de modelos de IA e uma **WebUI** para interação via chat.

## 📌 Requisitos

- Docker e Docker Compose instalados no sistema.
- Conexão com a internet para baixar os modelos e dependências.

## 📦 Estrutura do Projeto

```
.
├── docker-compose.yml
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

### `docker-compose.yml`

```yaml

services:
  open-webui:
    container_name: open-webui
    image: ghcr.io/open-webui/open-webui:main
    environment:
      - MODEL_DOWNLOAD_DIR=/models
      - OLLAMA_API_BASE_URL=http://ollama:11434
      - OLLAMA_API_URL=http://ollama:11434
      - LOG_LEVEL=debug
    volumes:
      - data:/data
      - models:/models
      - open-webui:/config
    ports:
      - "8080:8080"
    logging:
      driver: json-file
      options:
        max-size: "5m"
        max-file: "2"
    depends_on:
      - ollama
    extra_hosts:
      - "host.docker.internal:host-gateway"
    networks:
      - ollama-net
    restart: unless-stopped

  ollama:
    container_name: ollama
    image: ollama/ollama:latest
    runtime: nvidia
    environment:
      - NVIDIA_VISIBLE_DEVICES=all
      - NVIDIA_DRIVER_CAPABILITIES=compute,utility
      - CUDA_VISIBLE_DEVICES=0
      - LOG_LEVEL=debug
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              capabilities: [gpu]
              count: all
    volumes:
      - ollama:/root/.ollama
      - models:/models
    ports:
      - "11434:11434"
    logging:
      driver: json-file
      options:
        max-size: "5m"
        max-file: "2"
    networks:
      - ollama-net
    restart: unless-stopped

volumes:
  data:
  models:
  ollama:
  open-webui:

networks:
  ollama-net:
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


### **Requisitos**
- **Docker** instalado e configurado.
- **Hardware**: Recomenda-se uma GPU compatível com CUDA para melhor desempenho.


### **Links Úteis**
- [Repositório do usrbinkat - Ollama + Open-Webui + Nvidia/CUDA + Docker + docker-compose](https://gist.github.com/usrbinkat/de44facc683f954bf0cca6c87e2f9f88)
- [Instalação do Docker](https://docs.docker.com/get-docker/)
- [NVIDIA Docker Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)

## 📝 Contribuição

Sinta-se à vontade para abrir **issues** e **pull requests** para melhorar o projeto! 🚀

## 📄 Licença

GNU General Public License v3.0
