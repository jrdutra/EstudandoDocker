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

Comando genérico: `docker run [options] IMAGE [command][args]`

Como opções podemos ter: 

**-i** = Iniciar interação com o container

**-t** = Iniciar com um terminal de comando (Digitando exit para sair)

**-d** = Iniciar o container em segundo plano

**-rm** = Remover o container após termino

**--name** = Fornecer um nome ao container e não deixando a cargo do docker

Você pode dar o comando com o nome da imagem e ele vai levantar um container com um nome aleatorio referente a essa imagem.

* Lista os containers

```
sudo docker ps
sudo docker ps -a
sudo watch docker ps
```

Onde o `docker ps` lista os ativos e o `docker ps -a` lista todos os containers, inclusive os parados.

# Exemplos

**Levantar o container e rodar o comando logo após**
sudo docker run debian cat /etc/issue.net

**Levandar o container e rodar o comando depois que o container subiu**
sudo docker run -it ubuntu bash
sudo apt-get update && sudo apt-get install nano


# EXERCICIOS

##Tarefa 1:

A tarefa consiste em você executar a ferramenta chamada nmap, para descoberta de serviços ou servidores em uma rede de computadores, e responsável por escanear host específico, a fim de saber se determinado serviço está disponível no mesmo.

A sintaxe, de comando nmap, a ser usada é:

```
nmap -sS [IP address]
```

Para realização dessa tarefa, você deve mostrar como executar essa ferramenta, em container Docker, usando os modos apresentados abaixo:

**Perguntas dessa tarefa**

**MODO 1** - Usando imagem, do Docker Hub, nomeada uzyexe/nmap

**Resposta** 

```
sudo docker run uzyexe/nmap nmap -sS 127.0.0.1
```

**MODO 2** - Instalando a ferramenta diretamente no container, de imagem Ubuntu:18.04, e executando-a a partir dele

**Resposta** 
```
sudo docker run -it ubuntu:18.04 bash
# nmap -sS 127.0.0.1
```


