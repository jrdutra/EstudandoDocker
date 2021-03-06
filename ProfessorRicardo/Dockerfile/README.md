# Notas de aulas do curso de Docker do professor Ricardo

Fonte do curso: [Udemy](https://www.udemy.com/course/docker-introducao-a-administracao-de-containers/)

## Criando Imagens

#### Docker Commit

Indicado para ambientes de teste

**Preciso ter:**

1. Um container temporário
2. Realizar a mudança
3. Consolidar a mudança
4. Novo container a partir da imagem


#### Estrutura do comando

```
sudo docker commit [OPTIONS] [CONTAINER] [REPO]
```
**[OPTIONS]** `--change, -c => Aplica instruções Dockerfile

**[REPO]** Adotar o formato `nomerepo/nomeimagem:versão`


##### Exemplo

Criar um container temporário:

```
sudo docker run -itd --name temp debian bash
```

Acessar o container utilizando o seguinte comando:

```
sudo docker exec -it temp bash
```

Instalar o apache por exemplo:

```
apt-get update && apt-get install apache2
```

Terminado o processo, digitar o comando `exit` para siar do container.

Devo parar o meu container com o comando:

```
sudo docker stop temp
```

Tenho que fazer o commit com o comando:

```
sudo docker commit temp jrdutra/debian_web:1.0
```

Ao executar o comando `sudo docker images` já posso ver a imagem comitada.

Agora eu posso criar outro container a partir dessa minha imagem, faço isso da seguinte forma:

```
sudo docker run --name minhaappweb -p 8081:80 jrdutra/debian_web:1.0 /usr/sbin/apache2ctl -D FOREGROUND
```

#### Docker Build (Dockerfile)

O dockerfile é como uma receita para criar uma imagem

O processo todo segue os seguintes passos:

1. Criar arquivo Dockerfile
2. Processo de build
3. Container podem ser criados usando a imagem criada

##### Exemplo

Devo criar um diretório para os meu arquivos com o comandoi:

```
mkdir Meudiretorio
```

Devo entrar no meu diretório

```
cd Meudiretorio
```

E então crio meu arquivo Dockerfile

```
nano Dockerfile
```

Já no meu arquivo eu insiro as diretiva como no exemplo:

```
FROM ubuntu:18.04
RUN apt-get update
RUN apt-get install apache2 -y
```

Salvo o arquivo...

No Meudiretorio onde já estou, rodo o docker build para criar a imagem a partir do Dockerfile criado. Uso o seguinte comando:

```
sudo docker build -t jrdutra/web2:2.0 .
```

Agora basta subir o container a partir da imagem criada.

```
sudo docker run -d --name web2 -p 8082:80 jrdutra/web2:2.0 /usr/sbin/apache2ctl -D FOREGROUND
```

##### Dockerfile Reference

A seguir algumas instruções para criação do Dockerfile

###### Instrução FROM

Configura a image base

**Exemplo**

```
FROM ubuntu:18.04
```

###### Instrução RUN

Pode obedecer o formado *shell form* (Usando barras para quebra de linha)

**Exemplo**
```
RUN apt-get update && \
apt-get install -y \
apache2 \
nano
```

Ou o formado *exec form*

```
RUN ["apt-get", "install", "apache2", "-y"]
```

###### Instrução COPY

Copia os arquivos do diretório atual do host (build context) (diretório onde está o Dockerfile) para dentro da imagem.

**Exemplo**

```
COPY . /app
```

Copia todos os arquivos do diretório atual do Dockerfile para dentro da pasta /app da imagem.

###### Instrução WORKDIR

Configura um diretorio de trablho. Esse diretório é onde o diretório raiz de trabalho do container. É o diretório onde o container se inicia.

**Exemplo**

```
WORKDIR /meudir
```

###### Instrução EXPOSE

É uma declaração de qual socket de trabalho. Por exemplo se tenho uma aplicação na imagem que roda na porta 8000, devo criar uma imagem com o `EXPOSE 8000`

**Exemplo**

```
EXPOSE 8000
```

###### Instrução VOLUME

Já mapeia um volume de disco para o futuro container docker trabalhar, me isentando de ter de criar um novo volume e passar essa instrução para o `docker run`

**Exemplo**

```
VOLUME /var/www/html
```

###### Instrução CMD

A instrução CMD, diferente da RUN, é executada quando o container dessa imagem é iniciado. O comando RUN é executado ao criar a imagem. O comando CMD é útil para colocar comando que devem ser executados após iniciar o container, por exemplo: 
Se você está fazendo o deploy de uma aplicação laravel, ao final do arquivo Dockerfile, você deve executar o comando para startar a aplicação laravel, que no caso ficaria como no exemplo:

*shell form*

```
CMD php artisan serve --host=0.0.0.0 --port=8000
```

*exec form*

```
CMD ["php","artisan","serve", "--host=0.0.0.0", "--port=8000"]
```

O mais utilizado para o comando CMD é o *exec form*.

O CMD executa apenas o comando que é especificado, quando eu faço o comando do `docker run....`, se eu passar parâmetros ao final do comando, o docker sobrescreve esse comando que estava no Dockerfile declarado com o CMD e passa a executar o comando que passei como parâmetro ao executar o `docker run...`.

###### Instrução ENTRYPOINT

É semelhante ao CMD, mas ele présupõe que você está no shell, o mesmo exemplo do CMD, ficaria assim com o Etrypoint:

```
ENTRYPOINT ["php","artisan","serve"]
```

No caso do Entrypoint, independente de eu passar ou não mais parâmetros ao final do comando `ocker run...`, o comando especificado no container não é sobrescrito.

#### Envio das imagens para o dockerHub

1 - Primeiro você deve criar um conta no site [dockerhub](https://hub.docker.com/)

2 -  Faça o login através do comando:

```
sudo docker login
```

ou

```
sudo docker login --username=jrdutra
```

3 - Devo rodar o comando docker push:

```
sudo docker push jrdutra/web2:2.0
```