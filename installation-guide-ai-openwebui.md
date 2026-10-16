#Instalação e configuração do modelo Qwen 2.5 1.5B através da WebUI para utilização no chatbot do website no Ubuntu.

#1.Recomenda-se instalar o Open WebUI no Ubuntu usando Docker, que é a forma mais estável e simples. Execute o seguinte comando para baixar e iniciar:
 #Passo 1: Confirmar se Docker e Docker Compose estão instalados, execute:
  docker --version
  docker compose version
 #Se ambos retornarem versão criar o docker-compose.yml.
 #Se nao execute:
 
 sudo apt update
 sudo apt install -y docker.io docker-compose-plugin
 
 #e depois, Habilitar o Docker para iniciar automaticamente com o sistema:
 
 sudo systemctl enable docker
 sudo systemctl start docker
 
 #verificar se esta ativo:
 
 sudo systemctl status docker
 
 #Se estiver ativo,
 
 #Passo2: Criar o arquivo docker-compose.yml no server e colar:
 services:
  ollama:
    image: ollama/ollama:latest #(motor de Inferencia)
    container_name: ollama
    volumes:
      - ollama:/root/.ollama
    restart: unless-stopped

  open-webui:
    image: ghcr.io/open-webui/open-webui:main
    container_name: open-webui
    volumes:
      - open-webui:/app/backend/data
    depends_on:
      - ollama
    ports:
      - "3000:8080"
    environment:
      - 'OLLAMA_BASE_URL=http://ollama:11434'
    restart: unless-stopped

volumes:
  ollama:
  open-webui:
  
#1. Digitar: 
sudo docker compose up -d
 
 
 #2 e depois do sudo docker compose up -d terminar vamos baixar o modelo do ollama
 sudo docker exec -it ollama ollama pull gemma3:1b
 
 #3 Confirmar que o modelo está disponível
  sudo docker exec -it ollama ollama list
  #gemma3:1b    97558784bdc9    806 MB    About a minute ago
  #de seguida abra o openWebUI
 
  
    
