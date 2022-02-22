# Docker
Docker do básico ao avançado

# Comandos Containers

Visualizar os containers quee estão em execussão

```docker ps```

Visualizar todos os containers

```docker ps -sa```

Parar um container específico

```docker stop CONTAINER ID```

Parar todos os containers que estão sendo executados

```docker stop $(docker container ls -q)```

Removendo todos os containers

```docker container rm $(docker container ls -aq)```

Iniciar o container de modo interativo
```docker run -it ubuntu bash```

Visualizar as camadas da imagem no container
```docker history ubuntu```

Visualizar o tamanho virtual do meu container que equivale ao tamanho total da imagem mais o tamanho total de dados do meu container. ex: SIZE = 16.2MB(virtual 89MB)
```docker ps -s```

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

Removendo todas as imagens
```docker rmi $(docker image ls -aq)```
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

# Persistindo informações

# Flag -v

Ao executar um container para criá-lo precisa passar paramentros para que os dados que forem persistidos local possem a ser persistido em algum diretório na mais máquina, no meu host. Basta criar o diretório neste caso criei um com o nome volume-docker e referencie-o, tudo que for criado no diretório do container /app será persistido no diretório /volume-docker do meu host.

```docker run -it -v /var/www/html/projetos/docker/volume-docker:/app ubuntu bash```

# Flag --mount 

```docker run -it --mount type=bind,source=/var/www/html/projetos/docker/volume-docker,target=/app ubuntu bash```

Flag volume mais usada

Mostrar os volumes criados

```docker volume ls```

Criando volume

```docker volume create meu-volume```

Iniciando o container com o volume criado

```docker run -it -v meu-volume:/app ubuntu bash```

Criando volume com --mount

```docker run -it --mount source=meu-volume,target=/app ubuntu bash```

# Flag tmpfs

Inciando o container com --tmpfs

```docker run -it --tmpfs=/app ubuntu bash```

--tmpfs com -- mount

```docker run -it --mount type=tmpfs,destination=/app ubuntu bash```

