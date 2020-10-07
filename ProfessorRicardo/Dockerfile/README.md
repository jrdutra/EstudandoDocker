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


#### Exemplo

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

