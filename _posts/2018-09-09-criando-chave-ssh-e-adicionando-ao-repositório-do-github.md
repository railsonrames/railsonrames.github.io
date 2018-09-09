---
layout: post
title: "Criando chave SSH e adicionando ao repositório do GitHub"
date: 2018-09-09 09:25:27
image: 'https://cdn-images-1.medium.com/max/1000/1*HN1Hw2ZHpndoRdfo9AHNPA.png'
description: Como criar um par de chaves SSH habilitando um caminho de autenticação entre uma máquina em específico ao GitHub.
category: 'DevOps'
tags:
- Segurança
- Git
twitter_text:
introduction: Aprenda como criar uma chave assíncrona para evitar a repetida autenticação em uma conta do GitHub em uma máquina que não está definida com parâmetros globais do Git.
---
# Qual motivo ou circunstância é interessante criar uma chave assíncrona?

Inicialmente o cenário, relato o meu, no caso de ter vários projetos em uma máquina (comumente), ocorre que tenho um projeto em C# que o Visual Studio 2017 gerencia, nele tenho permissões para o usuário Git do VSTS, perfeito. Entretanto, tenho vários outros projetos em C#, casos de estudo em sua maioria, e para esses configuro manualmente as opções de repositório, mas mais uma vez o Visual Studio cuida dessas preferências por mim. Restando então o que o Visual Studio não pode gerenciar, por óbvio projetos como este site, que nada mais é do que um repositório como qualquer outro hospedado no GitHub, uma pasta que possui um diretório oculto .git que possui as diretivas para organizar o versionamento, porém como dito anteriormente, não se têm uma aplicação, IDE, para esse tipo de gerenciamento, logo existem parâmetros globais que o Git proporciona, o caso mais claro é o das diretivas globais como o comando `git config --global user.name "fulanodetal"` ou se preferir definir o usuário apenas para um diretório como em `git config user.name "fulanodetal"` é plenamente possível. Algo inegável é a autenticação, nos passos anteriores é notório que você pode dar o nome de usuário que bem entender, mas em suma e principalmente no envio de suas alterações para o repositório, no caso o GitHub em um *push*, será solicitado o usuário e senha, caso não esteja definido por padrão no *bash* da máquina.

Isso é muito chato quando se precisa ganhar tempo e por consequência produtividade, então a ideia de máquina confiável e uma senha apenas por sessão é um alternativa excelente. Em resumo, você associa uma chave ao agente de controle, insere a senha **da chave** (a senha do repositório pode e recomendo que seja diversa) e toda operação que requisita usuário e senha será suprida por esse par de chaves público-privada que vai ser criada e consultada ao longo do processo de desenvolvimento. Vamos ao operacional.

Geração da chave, com o bash unix/like você conseguirá acessar a aplicação `ssh-keygen`, atualmente estou executando terminal *bash* com o auxílio do Git Bash para Windows, vamos ao comando.

```$ ssh-keygen -t rsa -b 4096 -C "seuemail@usadoparaautenticarno.github.com"```

> Será solicitado o nome do arquivo (chave), se simplesmente pressionar Enter será criado com o nome padrão *id_rsa* no diretório do usuário (/home/seuusuario/.ssh/id_rsa).

> De imediato será solicitada a senha para essa chave a saída será como a do *log* abaixo.

```
$ ssh-keygen -t rsa -b 4096 -C "umemail@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/DELL/.ssh/id_rsa): teste_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in teste_rsa.
Your public key has been saved in teste_rsa.pub.
The key fingerprint is:
SHA256:m6igK9+pJpn67ThhCwOGF5P68d6oobXUIEuA5LGyJMA umemail@gmail.com
The key's randomart image is:
+---[RSA 4096]----+
|oo .             |
|=E*              |
|*= o             |
|Ooo              |
|=+.o    S        |
|+o+o.  . o       |
|.*=+.o. o        |
|==+Boo.          |
|B*B=*            |
+----[SHA256]-----+
```

:warning: Lembrando que a senha para a chave não precisa ser a mesma da conta do GitHub.

Com a chave criada precisamos incluí-la ao `ssh-agent`, que gerenciará quantas chaves tivermos, com o comando abaixo inciciamos o processo do agente.

```
eval $(ssh-agent -s)
Agent pid 99999
```

E agora só incluir a chave criada ao agente, com o comando.
```
ssh-add ~/.ssh/id_rsa
```

O negócio agora é associar a chave ao GitHub, para isso observe que não foi criado apenas o arquivo id_rsa, mas também o seu par público *id_rsa.pub* (o nome que você tenha dado .pub). É ele que vai ser usado para o GitHub, com o comando de cópia para a área de transferência você será capaz de copiar o conteúdo do arquivo de maneira fácil, segue abaixo.

```
clip < ~/.ssh/id_rsa.pub
```

:rotating_light: **clip** advém de *clipboard*, em portugês o equivalente a área de transferência.

Após fazer login no seu GitHub, vá as configurações, SSH and GPG keys e clique em *New SSH key*.

![Tela de configurações](https://uploaddeimagens.com.br/images/001/603/622/original/tela-de-configuracoes-do-git-em-ssh-e-gpg-keys.png?1536490881 "configrações de conta GitHub")

Na sequência colando o conteúdo armazenado no *clipboard* na seguinte tela.

![Tela de inclusão de chave](https://uploaddeimagens.com.br/images/001/603/625/full/tela-de-configuracoes-do-git-em-ssh-e-gpg-keys-adicionar.PNG?1536491334 "inclusão de chave pública")

E para confirmar será solicitada a sua senha do GitHub na página deles por óbvio.

![Tela de confirmação por senha](https://uploaddeimagens.com.br/images/001/603/627/full/tela-de-configuracoes-do-git-em-ssh-e-gpg-keys-adicionar-confirmar.PNG?1536491505 "confirmação com senha do GitHub")

Agora já podemos ver a chave incluída e aguardando por seu uso.

![Tela de lista de chaves cadastradas](https://uploaddeimagens.com.br/images/001/603/629/full/tela-de-configuracoes-do-git-em-ssh-e-gpg-keys-adicionar-confirmar-listagem.PNG?1536491648 "listagem de chaves cadastradas")

Porém ainda existe mais uma alteração a ser feita antes do derradeiro push.

Antes de utilizar a chave é importante mudar a URL do repositório remoto para https, para conferir basta usar o comando:

```
git remote -v

origin  https://github.com/railsonrames/seurepo.github.teste (fetch)
origin  https://github.com/railsonrames/seurepo.github.teste (push)
```

########################################

Additionally, you may want to add some keys at session start.

Edit your ~/.bashrc file, and add :

ssh-add &>/dev/null || eval `ssh-agent` &>/dev/null  # start ssh-agent if not present
[ $? -eq 0 ] && {                                     # ssh-agent has started
ssh-add ~/.ssh/your_private.key1 &>/dev/null        # Load key 1
ssh-add ~/.ssh/your_private.key2 &>/dev/null        # Load key 2
}
Check your keys with ssh-add -l

You can stop the current ssh-agent session with ssh-agent -k

Something to know about ssh-agent and .bashrc is don't load too many keys. The default number of tries for ssh daemon is limited to 6. This can been modified in /etc/ssh/sshd_config with the MaxAuthTries value.