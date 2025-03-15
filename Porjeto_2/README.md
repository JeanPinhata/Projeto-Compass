# **Fáceis** 
1- Rodando um container básico
Execute um container usando a imagem do Nginx e acesse a página padrão no navegador.

🔹 Exemplo de aplicação: Use a landing page do TailwindCSS como site estático dentro do container.

Para rodar um conteiner básico usando a imagem do Nginx utilizamdos o seguinte comando: 

```docker run -d -p 8080:80 --name meu-nginx nginx```

Para verificar o funcionamento abrimos o navegador e acessarmos o link http://localhost:8080

Criando os arquivos para hospedar a landingpage do TailwindCSS com o site estático dentro do conteirer com os seguintes comandos. 

``` mkdir meu-site && cd meu-site
 curl -o tailwind.js https://cdn.tailwindcss.com
 docker run --name meu-nginx-static -d -p 8080:80 -v $(pwd):/usr/share/nginx/html nginx
 ```

2- Criando e rodando um container interativo
Inicie um container Ubuntu e interaja com o terminal dele.

🔹 Exemplo de aplicação: Teste um script Bash que imprime logs do sistema ou instala pacotes de forma interativa.

Comando para rodar um contêiner no Ubuntu que seja interativo

```
docker run -it --name meu-ubuntu ubuntu bash
apt update && apt install -y curl
nano logs.sh
```
*Script Logs.sh*
```#!/bin/bash

# Diretório do host que será montado no container
HOST_DIR="/home/usuario/dados"

# Diretório dentro do container onde o diretório do host será montado
CONTAINER_DIR="/dados"

# Nome da imagem Docker que será usada
IMAGE_NAME="minha_imagem"

# Executa o container com a montagem do diretório
docker run -v "$HOST_DIR":"$CONTAINER_DIR" "$IMAGE_NAME"

echo "Container executado com o diretório '$HOST_DIR' montado em '$CONTAINER_DIR'."
```
Comandos para automatizar o script e visualização. 

```
chmod +x logs.sh
./logs.sh
   ```
3- Listando e removendo containers
Liste todos os containers em execução e parados, pare um container em execução e remova um container específico.

🔹 Exemplo de aplicação: Gerenciar containers de testes criados para verificar configurações ou dependências.

Para listar contêineres em execução:
```docker ps```

Para listar todos os contêineres existentes: 
``` docker ps -a```

Para interromper a execução de um contêiner:
```docker stop nome_do_container```

Para excluir um contêiner: 
``` docker rm nome_do_container```

4- Criando um Dockerfile para uma aplicação simples em Python
Crie um Dockerfile para uma aplicação Flask que retorna uma mensagem ao acessar um endpoint.

🔹 Exemplo de aplicação: Use a API de exemplo Flask Restful API Starter para criar um endpoint de teste.

```mkdir flask-app && cd flask-app```

Agora dentro do script app.py

```
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
return {"message": "Flask funcionando perfeitamente!"}

if __name__ == "__main__":
app.run(host="0.0.0.0", port=5000)
```
Dentro da pasta flask-app criaremos o arquivo ```nano script``` para armazenar o script. 

```
echo "from flask import Flask
app = Flask(__name__)
@app.route('/')
def home():
   return 'Olá, Docker!'
if __name__ == '__main__':
   app.run(host='0.0.0.0', port=5000)" > app.py
```

Criando o Dockerfile com o seguinte conteúdo:

```
echo "FROM python:3.9
WORKDIR /app
COPY app.py .
RUN pip install flask
CMD ['python', 'app.py']" > Dockerfile
```
Comando para criar a imagem e rodar o conteiner:

```
docker build -t flask-app
docker run -d -p 5000:5000 flask-app
```
Porta para acessar e visualizar a imagem criada: 
``` http://localhost:5000```

# *Médios*

5- Criando e utilizando volumes para persistência de dados
Execute um container MySQL e configure um volume para armazenar os dados do banco de forma persistente.

🔹 Exemplo de aplicação: Use o sistema de login e cadastro do Laravel Breeze, que usa MySQL.

```
 docker volume create mysql_data
 docker run -d --name meu-mysql -e MYSQL_ROOT_PASSWORD=root -v mysql_data:/var/lib/mysql mysql:latest

 docker run -d \
 --name mysql-container \
 -e MYSQL_ROOT_PASSWORD=rootpassword \
 -e MYSQL_DATABASE=laravel \
 -e MYSQL_USER=laravel_user \
 -e MYSQL_PASSWORD=laravel_password \
 -p 3306:3306 \
 -v mysql_data:/var/lib/mysql \
  mysql:latest

docker exec -it meu-mysql mysql -u root -p
```

6- Criando e rodando um container multi-stage
Utilize um multi-stage build para otimizar uma aplicação Go, reduzindo o tamanho da imagem final.

🔹 Exemplo de aplicação: Compile e rode a API do Go Fiber Example dentro do container.

```
package main

import (
"github.com/gofiber/fiber/v2"
)

func main() {
  app := fiber.New()

app.Get("/", func(c *fiber.Ctx) error {
     return c.SendString("Hello, Fiber!")
 })

app.Listen(":3000")
}
```
Criando o Dockerfile

```
# Etapa 1: Build (usando a imagem do Go para compilar)
FROM golang:1.20 AS builder

# Definindo o diretório de trabalho
WORKDIR /app

# Copiar os arquivos do projeto para o diretório de trabalho
COPY . .

# Baixar as dependências do Go (vai usar go.mod e go.sum)
RUN go mod tidy

# Compilar o aplicativo Go
RUN go build -o app .

# Etapa 2: Imagem final (imagem menor para rodar a aplicação)
FROM ubuntu:22.04

# Copiar o binário compilado da etapa anterior
COPY --from=builder /app/app /app

# Expor a porta em que a aplicação vai rodar
EXPOSE 3000

# Comando para rodar a aplicação
CMD ["/app"]
```
Criação do arquivo nano **main.go**

```
go mod init fiber-api
go get github.com/gofiber/fiber/v2
```
Comandos para criar a imagem e executar o conteneir:

```
docker build -t go-fiber-api .
docker run -p 3000:3000 go-fiber-api
```
Para testar a aplicação rode em seu servidor http://localhost:3000

7- Construindo uma rede Docker para comunicação entre containers
Crie uma rede Docker personalizada e faça dois containers, um Node.js e um MongoDB, se comunicarem.

🔹 Exemplo de aplicação: Utilize o projeto MEAN Todos para criar um app de tarefas usando Node.js + MongoDB.

Criação do Dockerfile para o conteiner Node.js

```
# Usar imagem oficial do Node.js
FROM node:16

# Definir o diretório de trabalho dentro do container
WORKDIR /app

# Copiar o package.json e o package-lock.json para o diretório de trabalho
COPY package*.json ./

# Instalar as dependências da aplicação
RUN npm install

# Copiar o restante dos arquivos da aplicação
COPY . .

# Expor a porta onde o app vai rodar
EXPOSE 3000

# Rodar a aplicação quando o container iniciar
CMD ["npm", "start"]
```
Criação de um docker-compose.yml ( nano docker-compose.yml)

```
version: '3'

services:
  nodejs:
  build
   context: .
      dockerfile: Dockerfile
ports:
   - "3000:3000"
networks:
  - todos-network
environment:
  - MONGO_URI=mongodb://mongo:27017/todos
depends_on:
 - mongo
restart: always

mongo:
 image: mongo:latest
 ports:
  - "27017:27017"
networks:
 - todos-network
restart: always

networks:
todos-network:
driver: bridge
```
Criação um arquivo app.js( nano app.js)

```
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');

// Inicializando o Express
const app = express();

// Conexão com o MongoDB
mongoose.connect(process.env.MONGO_URI, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log('Conectado ao MongoDB'))
.catch(err => console.log('Erro de conexão com o MongoDB', err));

// Middleware
app.use(cors());
app.use(express.json());

// Definindo o modelo de Tarefa (todos)
const Task = mongoose.model('Task', new mongoose.Schema({
task: String,
completed: { type: Boolean, default: false }
}));

// Rotas
app.get('/tasks', async (req, res) => {
const tasks = await Task.find();
res.json(tasks);
});

app.post('/tasks', async (req, res) => {
const task = new Task({ task: req.body.task });
await task.save();
res.status(201).json(task);
});

app.put('/tasks/:id', async (req, res) => {
const task = await Task.findByIdAndUpdate(req.params.id, req.body, { new: true });
res.json(task);
});

app.delete('/tasks/:id', async (req, res) => {
await Task.findByIdAndDelete(req.params.id);
res.status(204).end();
});

// Iniciar o servidor
app.listen(3000, () => {
console.log('Server rodando na porta 3000');
});
```
Criação  do arquivo package.json( nano packge.json)

```
 {
   "name": "mean-todos",
   "version": "1.0.0",
   "description": "App de tarefas com Node.js e MongoDB",
   "main": "app.js",
   "scripts": {
   "start": "node app.js"
},
  "dependencies": {
  "cors": "^2.8.5",
  "express": "^4.17.1",
  "mongoose": "^5.9.25"
},
 "author": "",
 "license": "ISC"
}
```
No diretório onde estão docker-compose.yml, Dockerfile, app.js, e package.json, execute os seguintes comandos para construir e rodar os contêineres:

```
docker-compose up --build  
```

8-Criando um compose file para rodar uma aplicação com banco de dados
Utilize Docker Compose para configurar uma aplicação Django com um banco de dados PostgreSQL.

🔹 Exemplo de aplicação: Use o projeto Django Polls App para criar uma pesquisa de opinião integrada ao banco.

Criando diretório do projeto. 

```
mkdir django-polls-app
cd django-polls-app
python3 -m venv venv
source venv/bin/activate
pip install django
django-admin startproject polls_project .
```
Criando o app "polls" dentro do projeto Django

```
      # Usa a imagem oficial do Python
      FROM python:3.10

      # Define o diretório de trabalho no container
      WORKDIR /app

      # Copia os arquivos para dentro do container
      COPY requirements.txt .
      COPY . .

     # Instala as dependências
     RUN pip install --no-cache-dir -r requirements.txt

     # Expõe a porta 8000
     EXPOSE 8000

     # Comando para rodar a aplicação Django
     CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```
Criando o arquivo requisitos.txt 

```
Django
psycopg2-binary
```
Criação do arquivo docker-compose.yml para configurar os serviços Django e PostgreSQL:

```
version: '3.8'

 services:
db:
image: postgres:latest
container_name: postgres_db
restart: always
environment:
 POSTGRES_DB: django_db
 POSTGRES_USER: admin
 POSTGRES_PASSWORD: admin123
ports:
 - "5432:5432"
volumes:
 - postgres_data:/var/lib/postgresql/data

web:
  build: .
  container_name: django_app
  restart: always
  depends_on:
    - db
 ports:
    - "8000:8000"
environment:
     DATABASE_URL: postgres://postgres:postgres@db:5432/pollsdb
    
volumes:
 - .:/app

volumes:
  postgres_data:
```
Alterando a configuração do banco de dados para usar o PostgreSQL. 

```
DATABASES = {
'default': {
 'ENGINE': 'django.db.backends.postgresql',
 'NAME': 'django_db',
 'USER': 'admin',
 'PASSWORD': 'admin123',
 'HOST': 'db',
 'PORT': '5432',
 }
}
```
Comandos para subir a aplicação. 

```
docker-compose up -d
docker exec -it django_app python manage.py migrate
docker exec -it django_app python manage.py createsuperuser
```

# *Difícil* #

9- Criando uma imagem personalizada com um servidor web e arquivos estáticos
Construa uma imagem baseada no Nginx ou Apache, adicionando um site HTML/CSS estático.

🔹 Exemplo de aplicação: Utilize a landing page do Creative Tim para criar uma página moderna hospedada no container.

Criar a pasta para o projeto 

```
mkdir -p ~/meu-site-nginx
cd ~/meu-site-nginx
```
É preciso baixar e extrair os arquivos do site na pasta *~/meu-site-nginx*

Criando o Dockerfile com o seguinte conteúdo:

```
nano Dockerfile

FROM node:18 AS build

WORKDIR /app


COPY . .


RUN npm install && npm run build


FROM nginx:latest


COPY --from=build /app/build /usr/share/nginx/html


EXPOSE 80
```
Comandos para contruir a imagem e rodar o conteiner. 

```
docker build -t meu-site-nginx .
docker run -d -p 8080:80 --name site-container meu-site-nginx
```
Para acessar use http://localhost:8080 em seu servidor.





