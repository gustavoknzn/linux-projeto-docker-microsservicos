## Ferramentas Utilizadas
- [Docker](https://www.docker.com/)
- [MySQL](https://www.mysql.com/)
- [NGINX](https://www.nginx.com/)
- [Loader.io](https://loader.io/)
- [AWS](https://aws.amazon.com/)
- [Ubuntu](https://ubuntu.com/)

## Instalação e configurações iniciais

Para facilitar o deploy da aplicação, foram criados scripts que estão na pasta "docker"

- execute na ordem os scripts __installdocker.sh__ e __docker_containers_create.sh__

#### Liberar acesso a porta 3306 do MySQL nas regras do firewall no EC2

## Iniciando o swarm
+ No servidor principal deverá ser executado o comando:

```sh
#docker swarm init
```

+ Nos clientes

Copiar o arquivo gerado com o token e ip

```sh
exempo: docker swarm join --token SWMTKN-1-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx 172.xx.xx.xxx:2377
```

## Replicando o servidor

#### Configurações do servidor principal

Execute o comando para replicar o servidor 
```sh
#docker service create --name web-server --replicas 10 -dt -p 80:80 --mount type=volume,src=app,dst=/app/ webdevops/php-apache:alpine-php7
```

Instalação do servidor NFS para compartilhar as configurações com os clientes:

```sh
#apt install nfs-server
```
Editar o arquivo */etc/exports* e adicionar o caminho abaixo na última linha

```sh
#nano /etc/exports
/var/lib/docker/volumes/app/_data *(rw,sync,subtree_check)
#exportfs -ar
```

#### Configurações dos servidores clientes

Instalação do cliente NFS para poder conectar ao servidor principal e receber as configurações:

```sh
#apt install nfs-common
```

Montar o diretório que foi previamente exportado no servidor principal
```sh
#mount -o v3 172.xx.xx.xxx:/var/lib/docker/volumes/app/_data /var/lib/docker/volumes/app/_data
```

## Configurando proxy com o NGINX

As configurações deverão ser setadas de acordo com o ip de cada EC2 e serão importadas pelo __dockerfile__

Criar uma nova imagem para utilizar o proxy

```sh
#docker build -t proxy-app .
```

Executar a imagem criada:

```sh
#docker container run --name my-proxy-app -dti -p 4500:4500 proxy-app
```

## Teste de Stress

- Acesse o loader.io 
- Realize as configurações para acesso
- Crie um novo teste para verificar a resposta do serviço


## [Meu LinkedIn](https://www.linkedin.com/in/gustavo-konzen-70373b189/)
