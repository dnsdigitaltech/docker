# docker
Docker do básico ao avançado

Para criar a imagem no docker
docker build -t developer/app-node:1.0 .

Visualizar a imagem criada
docker images

Rodar a aplicação da porta 3000 para minha porta 8081
docker run -d -p 8081:3000 developer/app-node:1.0

Basta acessar a aplicação em
http://localhost:8081/