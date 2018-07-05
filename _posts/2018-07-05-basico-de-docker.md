---
layout: post
title: "Conhecimentos básicos de Docker"
date: 2018-07-05 00:00:00
image: 'https://tr1.cbsistatic.com/hub/i/r/2017/03/23/9cf93159-d002-4d3b-b100-c0a49a4a3189/resize/770x/39d767be960faaa34ae565de17219d78/dockernewhero.jpg'
description: Precisei recorrer a consulta depois de muito tempo sem usar Docker e hoje precisei criar um container para montar um ambiente com node.js para um desafio json do curso de web moderno.
category: 'estudos'
tags:
- estudos
- tips
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
---

Executando o Docker Container
Conteúdo do ```./run```

Crie um arquivo com o nome run com o conteúdo abaixo, na mesma pasta e estrutura;
```
sudo docker run --name cpp-ghost -v  `pwd`/content:/var/lib/ghost -p 9001:2368 -d ghost
```
Desvendando o comando docker run:

```--name``` - alias utilizado para criação do Docker Container;
```-p``` - utilizado para fazer o port forwarding, a porta 9001 do host redireciona para porta 2368 do Docker Container, isto quer dizer que a magia começa por aqui, o acesso no http://localhost:9001 redireciona para o container;

```-d``` - indica qual imagem será utilizada, neste caso a imagem ghost;

```-v``` - indica o path para montagem do caminho virtual, assim podemos acessar os arquivos de conteúdo gerados dentro do container o alias pwd foi utilizado para tornar o caminho relativo em absoluto.

Execute os comandos abaixo para permitir e executar o arquivo run;
```
chmod +x run ./run
```

Esse trecho de texto foi retirado na íntegra do portal "Coisa de Programador" e pode ser acessado toda sua extensão em: <http://www.coisadeprogramador.com.br/criando-sua-primeiro-docker-container/>
