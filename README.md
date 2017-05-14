# Docker developer environment

Ambiente completo para desenvolvimento em containers Docker e Docker Compose para abstração da inicialização dos serviços.

Possui também a funcionalidade de incluir atalhos no **bash**(terminal interpretador de comandos) para facilitar a
execução de comandos presentes nos containers, diretamente no terminal do host hospedeiro. Isso permite executar alguns
comandos como **php, mysql, etc** de forma transparente e fora do container.

## Funcionalidades

Serviços e ferramentas disponibilizadas:

 * PHP-FPM
 * Nginx
 * MySQL

## Requisitos

 * Docker >= 1.12
 * Docker Compose >= 1.9

Exemplo de instalação do Docker e Docker Compose no Ubuntu:

```
$ sudo curl -sSL https://get.docker.com | sh

$ sudo curl -L "https://github.com/docker/compose/releases/download/1.9.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
```

## Utilização

Copiar o arquivo **.env.example** para **.env** e alterar as variáveis de ambiente conforme necessidade.

| Variável             | Descrição                |
|:-------------------- |:------------------------ |
| PHP_VERSION          | Versão do PHP (5.6 ou 7.1) |
| MYSQL_VERSION        | Versão do BD MySQL (5.6, 5.6.35, 5.7 ou latest) |
| MYSQL_DATA           | Diretório onde serão armazenados os dados do MySQL |
| MYSQL_ROOT_PASSWORD  | Senha do usuário **root** do MySQL |
| NGINX_HOSTS_CONF     | Díretório que pussui arquivos de configuração de virtualhosts(sites) do nginx |
| WWW_DATA             | Document root onde estão os arquivos dos sites |

Importante: o arquivo **.env.example** serve apenas para exemplo e não deve ser editado.

A configuração de proxy realizada nas variáveis de ambiente é para consumo dos containers.
Para que o Docker utilize proxy é necessário realizar configurações adicionais conforme documentação oficial:

https://docs.docker.com/engine/admin/systemd/#/http-proxy

Para facilitar a utilização da estrutura os principais comandos foram abstraidos em um Makefile.

A execução depende do programa make que é instalado por padrão na maior parte das distribuições Linux. Ex:
```
$ make <opcao>
```

### Lista de opções do Make:

| Opção     | Descrição                |
|:--------- |:------------------------ |
| bashrc    | Instala opções adicionas no BASH do usuário corrente para utilização de serviços por meio de containers de forma transparente. Ex: composer, npm, gulp, mongo, mysql, etc. |
| build     | Remove imagens buildadas anteriormente e recompila todos os containers. |
| up        | Inicia todos os serviços. Alguns containers são utilizados sob demanda e não são iniciados com esta opção. |
| stop      | Para os serviços em execução |
| clear     | Para os serviços em execução e remove todos os containers e imagens compiladas (não remove imagens base) |

### Opções útes do Docker:

```
$ docker ps                              # listar container em execução
$ docker stats                           # apresenta o consumo de recursos dos cantainers em tempo real
$ docker exec -it <container_name> bash  # acessa o terminal do container (algumas distribuições como alpine possuem apenas **sh**)
$ docker logs -f <container_name>        # observa logs do container
<Ctrl> + d ou exit                       # abandona o terminal
```

### Opções disponíveis após a instalação do "make bashrc":

**composer <options>**

Permite utilizar o gerenciador de pacotes do PHP conteinerizado. Ex:
```
$ composer install
$ composer update
```
**php**:

Permite utilizar o CLI do container php para executar pela linha de comando. Ex:
```
$ php -v
```

**mysql**:

Permite utilizar o CLI do container mysql para importação de arquivos SQL. Ex:
```
$ mysql -uroot -p123456  < db.sql
```