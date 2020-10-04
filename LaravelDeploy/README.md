
# Como colocar uma aplicação Laravel:5.7 em produção em um container

O primeiro passo para colocar uma aplicação laravel em produção, é desvincula-la do arquivo .env.

Isso pode ser feito passando as configurações do arquivo .env para alguns arquivos situados na pasta `./config` que fica na raiz do projeto.

* Acesse a pasta do projeto com o comando

```
cd /RaizDoProjeto
```

* Vá até a pasta `./config`, abra o arquivo `database.php` e substitua a seguinte linha:

```php
'default' => env('DB_CONNECTION', 'mysql'),
```

* Pela linha:

```php
//LOCAL
//'default' => env('DB_CONNECTION', 'mysql'),
//SERVIDOR
'default' => env('DB_CONNECTION', 'postegres'),

```


* Ainda no mesmo arquivo (`./config`) elimine a função .env das configurações do seu respectivo banco. se preferir, a função pode ser mantida, mas os parâmetros devem ser setados. no trecho de exemplo para o banco *mysql* ficaria assim:

```php
'mysql' => [
            'driver' => 'mysql',
            'host' => 'IP DO BANCO AQUI',
            'port' => '3306',
            database' => 'laravel_joao',
            'username' => 'seuusuario',
            'password' => 'suasenha',
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => 'utf8',
            'collation' => 'utf8_general_ci',
            'prefix' => '',
            'prefix_indexes' => true,
            'strict' => false,
            'engine' => null,
        ],
```

No caso do postgres ficaria assim com a função *env()*.

```php
        'pgsql' => [
            'driver' => 'pgsql',
            'host' => env('DB_HOST', 'IP DO BANCO AQUI'),
            'port' => env('DB_PORT', '5432'),
            'database' => env('DB_DATABASE', 'laravel_joao'),
            'username' => env('DB_USERNAME', 'suasenha'),
            'password' => env('DB_PASSWORD', ''),
            'charset' => 'utf8',
            'prefix' => '',
            'prefix_indexes' => true,
            'schema' => 'public',
            'sslmode' => 'prefer',
        ],
```

* Depois, dentro da mesma pasta, deve-se configurar o arquivo `app.php`. Indo até a chave 'KEY' e colocando a chave que está no campo 'APP_KEY' do arquivo `.env`.

Por exemplo, se no arquivo `.env``está assim:

```
APP_KEY=base64:21jtcVlnjAUgddIAgRQX1HNNCfiNAEPGv6mp9xgOhdg=
```

No arquivo  `app.php` deve ficar assim:

```
'key' => 'base64:21jtcVlnjAUgddIAgRQX1HNNCfiNAEPGv6mp9xgOhdg='
```

* O próximo passo é criar o arquivo Dockerfile e colocar na raiz do projeto...

### O arquivo Dockerfile

**Passos**:

1. Na raiz do seu projeto, deve-se criar um arquivo com o nome `Dockerfile` com o seguinte conteúdo:

```
FROM php:7.2.5-fpm

RUN apt-get update -y && apt-get install -y libmcrypt-dev openssl

RUN apt-get update && apt-get install -y libmcrypt-dev \
    && pecl install mcrypt-1.0.2 \
    && docker-php-ext-enable mcrypt

RUN docker-php-ext-install pdo mbstring

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN docker-php-ext-install pdo mbstring

RUN docker-php-ext-install mysqli pdo pdo_mysql

# Install Postgre PDO
RUN apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql

WORKDIR /app
COPY . /app
RUN composer install

CMD php artisan serve --host=0.0.0.0 --port=8000
EXPOSE 8000

```
**Esse Dockerfile serve tanto para aplicaçẽos que utilizem o mysql como banco quanto o postgres**

2. Abra um terminal e navegue até a raiz do seu proejto. `cd ./raizProjeto`

3. Jpa na raiz do projeto, execute o seguinte comando para criar a imagem do seu container:

```
sudo docker build -t imagemlaravel .
```

4. Após concluir o processo, deve-se criar o container com o seguinte comando:

```
sudo docker run -p 8000:8000 imagemlaravel
```

Lembrando que os passos 3 e 4 podem ser feitos pela interface gráfica do [Portainer](https://www.portainer.io/).

**Nesse ponto, a sua aplicação laravel deve estar rodando na porta 8000**

Fonte: [https://www.techiediaries.com/](https://www.techiediaries.com/docker-compose-laravel/)