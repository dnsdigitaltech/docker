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

Remobendo uma imagem

```docker rmi dnssites/books:1.0```

Removendo todas as imagens

```docker rmi $(docker image ls -aq)```

Removendo todas as imagens caso der erro

```docker rmi $(docker image ls -aq) --force```

### Subindo a imagem para o docker hub

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

### Dounload da imagem no docker hub

```docker pull dnssites/app-node:1.0``

# Persistindo informações

### Flag -v

Ao executar um container para criá-lo precisa passar paramentros para que os dados que forem persistidos local possem a ser persistido em algum diretório na mais máquina, no meu host. Basta criar o diretório neste caso criei um com o nome volume-docker e referencie-o, tudo que for criado no diretório do container /app será persistido no diretório /volume-docker do meu host.

```docker run -it -v /var/www/html/projetos/docker/volume-docker:/app ubuntu bash```

### Flag --mount 

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

### Flag tmpfs

Inciando o container com --tmpfs

```docker run -it --tmpfs=/app ubuntu bash```

--tmpfs com -- mount

```docker run -it --mount type=tmpfs,destination=/app ubuntu bash```

# Comunicação de containers pela rede

### Conhecendo a rede bridge

crie um conteianer em execução

```docker run -it ubuntu bash```

Abra um outro terminal e veja o conteiner em execução

```docker ps```

Inspeciona o container criado

```docker inspect 49f695aca8f3```

Você verá que vai aparece toda descrição do container inclusive o opção de bridge

"Networks": {
            "bridge": {
                "IPAMConfig": null,
                "Links": null,
                "Aliases": null,
                "NetworkID": "c4a9c2484d89fedb21bcc5eb19acc92aad924c2f2204162766e4be0d16e9cd4f",
                "EndpointID": "c0d7d796ae3527b15b58498a71c7590ef0a2f6e8694ead59eabe7dc76109295b",
                "Gateway": "172.17.0.1",
                "IPAddress": "172.17.0.2",
                "IPPrefixLen": 16,
                "IPv6Gateway": "",
                "GlobalIPv6Address": "",
                "GlobalIPv6PrefixLen": 0,
                "MacAddress": "02:42:ac:11:00:02",
                "DriverOpts": null
            }
        }

OBS: quem configuou esta rede foi o próprio docker!

crie um novo conteianer em execução

```docker run -it ubuntu bash```

Inspecione os containers criados

```docker inspect 0629463ea98b```
```docker inspect 49f695aca8f3```

Verificando as redes criada pelo docker

```docker network ls```

Comunicação entre IPs dos containers

```ping 172.17.0.2```

### Criando a rede bridge

Criando a proprio bridge para configurar o host name do nosso container

```docker network create --driver bridge minha-bridge```

Definindo o próprio nome do container para acessar ele via rede
```docker run -it --name ubuntu1 --network minha-bridge ubuntu bash```

Abra um outro terminal e veja o conteiner em execução

```docker ps```

Inspeciona o container criado

```docker inspect ea101feb2759```

OBS: Veja que agora esta com o nome da bridge criada

"Networks": {
                "minha-bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": [
                        "ea101feb2759"
                    ],
                    "NetworkID": "155061d96b606584b95396d964c1f2d3e6f2dc86ad95dad4bf6f6589ef9e361f",
                    "EndpointID": "c37a6a5742701a495cd3b6bac01f7424a89018c67adc3aa83f607a297ce68be3",
                    "Gateway": "172.18.0.1",
                    "IPAddress": "172.18.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:12:00:02",
                    "DriverOpts": null
                }
            }

Crie um novo containa com execução de 1 dia

```docker run -d --name ubuntu2 --network minha-bridge ubuntu sleep 1d```

Comunicação entre nomes dos containers

```ping ubuntu2```

### As redes none e host

Criando container rede none

```docker run -d --network none ubuntu sleep 1d```

Verificando os drivers existentes

```docker network ls```

Executando o container na rede host

```docker run -d --network host dnssites/app-node:1.0```

Neste caso se a minha aplicação funciona na porta 3000 tenho acesso direto, pois coloquei meu container no mesmo host da minha rede.
 ```http://localhost:3000/```

 OBS: se houvesse outra aplicação rodadno na mesma porta daria conflito e não executaria este container.

### Comunicando aplicação e banco

Baixe a imagem do banco mongo:4.4.6.

```docker pull mongo:4.4.6```

Baixe a imagam da aplicação

```docker pull dnssites/books:1.0```

Crie container do banco mongo:4.4.6 na bridge minha-bridge, para que haja comunicação interna entre containers e com o host name meu-mongo, pois a aplicação está setada com este host name.

```docker run -d --network minha-bridge --name meu-mongo mongo:4.4.6```

Crie container da aplicação na bridge minha-bridge, para que haja comunicação interna entre containers e com o host name books, com o mapeamento de porta 3000.

```docker run -d --network minha-bridge --name books -p 3000:3000 dnssites/books:1.0```

Para acessar a aplicação basta colocar

http://localhost:3000/

http://localhost:3000/seed

Para e exetutar o container do banco mongo

```docker stop meu-mongo```

```docker start meu-mongo```

# Cordenando containers

### Conhecendo o docker-compose

Instalando o docker-compose

```https://docs.docker.com/compose/install/```

### Definindo serviços

Criando o arquivo docker-compose.yml

OBS: está em ymls/docker-compose.yml

Executando ao arquivo docker-compose.yml

```docker-compose up```

### Complementando o Compose

Documentação completa

```https://docs.docker.com/compose/compose-file/compose-file-v3/```

Inicia sem travar o terminal

```docker-compose up -d```

Mostra os serviços que foram criados pelo docker-compose

```docker-compose ps```

Remove todos os componentes e redes que foram criados

```docker-compose down```

