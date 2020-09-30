# laradock-env
Laradock With Personal Settings

# Instruções

1. Crie um diretório para seus projetos
2. Clone esse repositório para ele:
```
   git clone git@github.com:cpereiraweb/laradock-env.git .
```
3. Clone o Laradock nesse diretório e crie manualmente a pasta `.laradock` para armazenar os dados dos contêineres:
```
   git clone https://github.com/laradock/laradock.git
   md .laradock
```

4. Copie o arquivo `copy-this-file-as-env-to-laradock-folder` para a pasta `laradock` como `.env`:
```
copy copy-this-file-as-env-to-laradock-folder laradock\.env
```
  - (opcional) Edite o arquivo `.env` e altere o nome do seu projeto Laradock:
```
  COMPOSE_PROJECT_NAME=laradock
```
5. Edite o arquivo `hosts` do seu computador e crie as seguintes entradas:
```
127.0.0.1 mysql
127.0.0.1 redis
127.0.0.1 phpmyadmin
```
Crie também as entradas para os virtualhosts do Nginx dos seus projetos (uma entrada para cada projeto), sempre com a TLD `.test`:
```
127.0.0.1 meuprojeto.test
```
6. Crie as configurações dos seus projetos em `laradock\nginx\sites`.  Use o arquivo `laravel.conf.example` como base e faça os ajustes necessários.
**Observação: a sua pasta de projetos equivalerá à `/var/www` no container**
<pre>
    server_name <strong>meuprojeto.test</strong>;
    root /var/www/<strong>meuprojeto</strong>/public;

</pre>
Salve esse arquivo como **meuprojeto.conf**

7. Acesse a pasta `laradock` e inicialize os contêineres para o ambiente:
```
cd laradock
docker-compose up -d nginx mysql redis phpmyadmin
```
8. Shells disponíveis:
- `docker-compose exec mysql bash`: abre o bash na instância do MySQL
- `docker-compose exec workspace bash`: abre o bash no contâiner no diretório `/var/www`
9. Databases para os projetos:
 1. Edite o arquivo `mysqldbfordocker.projetos` e acrescente o nome dos databases necessários, um por cada linha:
```
...
meuprojeto
meuprojeto2
outroprojeto
...
```
 2. Dentro do `workspace`, execute o script `create-mysql-for-docker-projects`:
 ```
 ./create-mysql-for-docker-projects
 ```
 3. Nos arquivos `.env` dos seus projetos, informe as credenciais:
 ```
    DB_CONNECTION=mysql
    DB_HOST=mysql
    DB_PORT=3306
    DB_DATABASE=meuprojeto
    DB_USERNAME=root
    DB_PASSWORD=root
 ```
10. Predefinições:
- PHP 7.3
- Nginx (última versão)
- MySQL 5.7 - Usuário `root` e senha `root`
- REDIS (última versão)
