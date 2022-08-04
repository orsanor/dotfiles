# My Dot files:

## Automate this
```sh
### Instalando Git e coisas básicas

###Instalando o Chocolatey
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

choco feature enable -n=allowGlobalConfirmation

### Setup dos dotfiles
git clone https://github.com/orsanor/dotfiles.git ~/.dotfiles
cd ~/.dotfiles
ln -s ~/.dotfiles/.gitconfig ~/.gitconfig

### Finalização
cd ~ && mkdir ./dev

##
version: '3.7'
services:
  api:
    build:
      context: ./api
      target: develop
    ports: 
      - 3000:3000
    volumes:
      - ./api:/app
    depends_on:
      - postgres
      - redis
      - rabbitmq
  dashboard:
    build:
      context: ./dashboard
      target: develop
    ports: 
      - 80
    environment:
      - API_BASE_URL=http://localhost:3000
    volumes:
      - ./dashboard:/app
    depends_on:
      - api
  web:
    build:
      context: ./web
      target: develop
    environment:
      API_BASE_URL: https://api.sandbox.bancoafro.com.br
    ports: 
      - 3000
    volumes:
      - ./web:/app
    # depends_on:
    #   - api
  notification:
    build:
      context: ./notification
      target: develop
    ports: 
      - 3001:3000
    volumes:
      - ./notification:/app
  postgres:
    image: postgres:10-alpine
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports: 
      - 5432:5432
    environment:
      POSTGRES_USER: root
      POSTGRES_PASSWORD: root
      POSTGRES_DB: bancoafro
  redis:
    image: redis:5-alpine
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.8.0
    environment:
      - discovery.type=single-node
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - elasticsearch_data:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
  kibana:
    image: docker.elastic.co/kibana/kibana:7.8.0
    environment:
      ELASTICSEARCH_HOSTS: http://elasticsearch:9200
    ports:
      - 5601:5601
  neo4j:
    image: neo4j:4.2
    volumes:
      - neo4j_data:/data
    environment:
      - NEO4J_AUTH=none
  rabbitmq:
    image: rabbitmq:3-management-alpine
    ports:
      - 15672
    volumes:
      - rabbitmq_data:/var/lib/rabbitmq

  compredopequeno:
    build:
      context: ./compredopequeno
      target: develop
    volumes:
      - ./compredopequeno:/app
    ports:
      - 80

volumes:
  elasticsearch_data:
  postgres_data:
  neo4j_data:
  rabbitmq_data:

  ##
  
