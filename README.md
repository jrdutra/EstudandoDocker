# Iniciando com Docker

O Docker é uma plataforma open source que facilita a criação e administração de ambientes isolados. Ele possibilita o empacotamento de uma aplicação ou ambiente dentro de um container, se tornando portátil para qualquer outro host que contenha o [Docker](https://www.docker.com/) instalado.

Fonte: [TreinaWeb](https://www.treinaweb.com.br/blog/no-final-das-contas-o-que-e-o-docker-e-como-ele-funciona/#:~:text=O%20Docker%20%C3%A9%20uma%20plataforma,que%20contenha%20o%20Docker%20instalado.)

## Passos para instalação do Docker 

1. Antes de instalar pacotes docker, atualize o repositório em seu sistema e
atualize os pacotes.

```
sudo apt update
```
```
sudo apt upgrade
```

2. Agora instale o docker usando o comando apt abaixo.
```
sudo apt install docker.io -y
```

3. Após a conclusão da instalação, inicie o serviço docker e habilite-o para ser
iniciado sempre que o sistema for inicializado.

```
systemctl start docker
```
```
systemctl enable docker
```

4. Docker instalado no servidor ubuntu 16.04, verifique-o usando o comando
abaixo.

```
sudo docker version
```

## Passos para instalação do Portainer

O Portainer pode ser instalado em um contêiner docker ou separadamente sem
um contêiner docker. Neste tutorial, instalaremos o Portainer como um
contêiner [Docker](https://www.docker.com/). É realmente simples de instalar e executar em qualquer
sistema, pois só precisamos garantir o suporte do sistema para Docker.

1. Antes de instalar o Portainer, baixe a imagem do Portainer do DockerHub
usando o comando docker pull abaixo.

```
sudo docker pull portainer/portainer
```
2. Agora execute o Portainer usando o comando docker abaixo:

```
sudo docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer
```

4. O Portainer agora está sendo executado como um contêiner, verifique-o
usando o comando abaixo:

```
sudo docker ps
```
5. Acessar o seguinte link na sua máquina: *http://localhost:9000*
