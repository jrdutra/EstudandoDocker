# Notas de aulas do curso de Docker do professor ricardo

Fonte do curso: [Udemy](https://www.udemy.com/course/docker-introducao-a-administracao-de-containers/)

## Arquitetura

A arquitetura possui 4 elementos

* Docker Engine
* Docker Client
* Docker Objects
* Docker Registry

**1 - Docker Engine**

É o motor do docker

**2 - Docker Cliente**

Responsável por receber as entradas do usuário, é onde o usuário coloca os comandos do Docker

**3 - Docker Objects**

* Imagens: é um template básico para um containers
* Containers: é o consumo das imagens
* Network: É o objeto que garante a capacidade de comunicação dos containers
* Storage: É o objeto de persistencia dos dados do container

**4 - Docker Registry**

É onde as imagens estão salvas na nuvem, o repositório mais utiliado é o docker hub. Podendo ser público ou privado, onde você pode rodar um repositório local.

## Comandos e suas utilidades

* Listar imagens:

```
sudo docker images
```

* Baixar imagens | Fonte das imagens: https://hub.docker.com

```
sudo docker pull ubuntu:18.04
```

* Rodar um container:

```
sudo docker run <nome_na_imagem>
```

Comando genérico: `docker run [options] IMAGE [command][args]

Como opções podemos ter: 

**-i** = Iniciar interação com o container
**-t** = Iniciar com um terminal de comando (Digitando exit para sair)
**-d** =0,Iniciar o container em segundo plano
**-rm** = Remover o container após termino 

Você pode dar o comando com o nome da imagem e ele vai levantar um container com um nome aleatorio referente a essa imagem.

* Lista os containers

```
sudo docker ps
sudo docker ps -a
```

Onde o `docker ps` lista os ativos e o `docker ps -a` lista todos os containers, inclusive os parados.


