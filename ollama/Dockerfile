FROM ubuntu:22.04

# Instala dependências
RUN apt update && apt install -y curl && rm -rf /var/lib/apt/lists/*

# Baixa e instala o Ollama
RUN curl -fsSL https://ollama.ai/install.sh | bash

# Baixa um modelo base (ex: deepseek)
RUN ollama pull deepseek

# Exposição da API
EXPOSE 11434

CMD ["ollama", "serve"]
