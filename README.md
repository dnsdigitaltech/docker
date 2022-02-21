# docker
Docker do básico ao avançado

# comandos básicos

Visualizar os containers quee stão em execussão

```docker ps```

Parar um container específico

```docker stop CONTAINER ID```

Parar todos os containers que estão sendo executados

```docker stop $(docker container ls -q)```

# imagem

Para criar a imagem no docker

```docker build -t developer/app-node:1.0 .```

Visualizar a imagem criada

```docker images```

Executar a aplicação da porta 3000 para minha porta 8081

```docker run -d -p 8081:3000 developer/app-node:1.0```

Executar a aplicação sem mostrar a porta
```docker run -d developer/app-node:1.0```

Basta acessar a aplicação em

http://localhost:8081/