---
layout: post
title: "Como mudar a senha padrão do usuário PostgresSQL"
date: 2018-10-05 14:50:47
image: 'http://uploaddeimagens.com.br/images/001/655/368/original/artigo-mudar-senha-padrao-postgres.png?1538761621'
description: Alterando a senha do usuário postgres.
category: 'DevOps'
tags:
- Postgres
- Banco de dados
twitter_text:
introduction: Aprendenda como mudar a senha do usuário padrão do Postgres de forma simples.
---

# Como mudar a senha padrão do usuario postgres no PostgresSQL?

Primeiramente é preciso logar com o usuário postgres na base de dados.

Por padrã a senha é ```postgres```.
```
$ /opt/PostgreSQL/8.4/bin/psql -U postgres -d postgres
ou no Windows
$ /c/PostgresSQL/pg10/bin/psql -U postgres -d postgres
```

Uma vez conectado é só rodar o comando de DDL para mudar a senha do usuário.

![Imagem esquema linguagens para SQL](http://3.bp.blogspot.com/-Ze_9mHCWfkM/UjSCEbcc3UI/AAAAAAAAAYQ/TOJQ3WlIlaI/s1600/DDL-vs-DML.png "Esquema de comandos SQL")

```
postgres=# ALTER USER postgres WITH PASSWORD ‘suanovasenhaaqui’;
ALTER ROLE
postgres=#
Pronto! Senha alterada!
```

Agradecimento e fonte ao site do [Emídio Leite](http://www.emidioleite.com.br/2013/09/27/mudar-a-senha-padrao-do-usuario-postgres-no-postgressql/), acessado em 5 de outubro de 2018.