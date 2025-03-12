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