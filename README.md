# docker-ollama-webui
## Configuração Docker para criar um servidor Ollama e WebUI de Chat

Servidor Ollama: Usa Ubuntu 22, instala o Ollama e um modelo base, e expõe a porta da API.
WebUI de Chat: Outra imagem para a interface do chat, conectada ao servidor Ollama.

Para executar o Docker:

No diretório docker-ollama-webui, execute:

```bash
docker-compose up -d
```
Isso iniciará o Ollama e a WebUI de chat automaticamente. 🚀
