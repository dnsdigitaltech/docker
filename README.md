# Docker
Docker do básico ao avançado

# Comandos básicos

Visualizar os containers quee stão em execussão

```docker ps```

Parar um container específico

```docker stop CONTAINER ID```

Parar todos os containers que estão sendo executados

```docker stop $(docker container ls -q)```

# Imagem

Para criar a imagem no docker

```docker build -t developer/app-node:1.0 .```

Visualizar as imagens criadas

```docker images```

Executar a aplicação da porta 3000 para minha porta 8081

```docker run -d -p 8081:3000 developer/app-node:1.0```

Executar a aplicação sem mostrar a porta
```docker run -d developer/app-node:1.0```

Basta acessar a aplicação em

http://localhost:8081/

# Subindo a imagem para o docker hub

Para acessar pleo terminal basta colocar o comando abaixo com seu login e senha

```docker login -u dnssites```

Visualizar as imagens e ecolher a que subirá

```docker images```

Enviando a imagem, basta dar o comando abaixo com o nome da sua imagem

OBS: é necessário subir para seu usuário neste caso será necessário criar um docker tag

```docker push developer/app-node:1.0```

Para criar um docker tag basta executar o comando abaixo com usuario/imagem do docker hub

```docker tag developer/app-node:1.0 dnssites/app-node:1.0```

```docker push dnssites/app-node:1.0```