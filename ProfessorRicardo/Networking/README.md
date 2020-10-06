# Notas de aulas do curso de Docker do professor ricardo

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

