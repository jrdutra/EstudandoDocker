# Notas de aulas do curso de Docker do professor Ricardo

Fonte do curso: [Udemy](https://www.udemy.com/course/docker-introducao-a-administracao-de-containers/)

## Networking (Redes dos containers)

Tipos:

1. Bridge
2. Host
3. none

Plugins drivers:

1. Bridge
2. Overlay
3. Macvlan

### Modo Bridge

* É a rede padrão que todo container recebe.
* Faz a ponte entre o host e os containers.

É criada uma subnet PRIVADA com um DHCP interno

**Esquema padrão de rede:**

![Esquema padrão de rede](https://raw.githubusercontent.com/jrdutra/EstudandoDocker/master/ProfessorRicardo/Networking/1.png)

**Para inspecionar:**

```
sudo docker network inspect bridge
```

### Nat port Forwarding / Port Mapping

* 1 porta do host <==> 1 Porta do container
* Mapeamento manual `-p` ou automático `-P`

Exemplos:

```
-p 8080:80
-p 192.168.1.100:8080:80
-p 8080:80/udp
-p 8080:80/tcp -p 8080:80/udp
```

Exemplo de criação de um container com mapeamento de portas que roda o apache (imagem httpd):

```
sudo docker run -d --name c1_apache -p 9090:80 httpd
```

Nesse caso, para acessar o container através do host, eu acesso como localhost:9090, essa conexão entra pela porta 9090 no docker e chega ao container pela porta 80.

### Network Driver Host

Nessa configuração não há isolamento de rede entre o host e o container. O container passa a funcionar como qualquer outro programa em localhost.

Comando para criar um container no modo driver host

```
docker run -itd -net host --name c1_busybox busybox
```

### Network drive none

Esse modo de rede desabilita completamente o acesso externo ao container.

Comando para criar um container no modo network drive none

```
docker run -itd -net none --name c1_busybox busybox
```