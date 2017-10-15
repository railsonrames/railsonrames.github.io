---
layout: post
title: "Uso do bloco using no C#"
date: 2017-10-14 21:12:26
image: 'http://ap.imagensbrasil.org/images/2017/10/14/2017-10-14-18_16_18-BDProjeto---Microsoft-Visual-Studio.png'
description: Entenda neste post como funciona o bloco using da linguagem C# e como o mesmo trabalha em conjunto com a CLR (Common Language Runtime).
category: 'tutorial'
tags:
- C#
- Método
- Learning
twitter_text:
introduction: Entenda neste post como funciona o bloco using da linguagem C# e como o mesmo trabalha em conjunto com a CLR (Common Language Runtime).
---

Entendendo o bloco Using no C#

Entenda neste post como funciona o bloco using da linguagem C# e como o mesmo trabalha em conjunto com a CLR (Common Language Runtime).

Quando programamos em C#, é bastante comum a prática e utilização do bloco using. Muitas pessoas o utilizam, e gostaria de falar um pouco sobre qual o objetivo deste bloco e como ele funciona dentro do .NET Framework.


O bloco using provê ao desenvolvedor a habilidade de se criar um bloco de código isolado dentro de um determinado programa. Podemos com ele, atingir dois objetivos:


·         Criar um alias para um determinado namespace ou tipo;

·         Criar um escopo que ao final de sua execução, libera recursos automaticamente através do método Dispose().


Exemplo:

using (System.Data.SqlClient.SqlConnection conn = new System.Data.SqlClient.SqlConnection())
{

}

No bloco acima, estamos fazendo referência ao namespace System.Data.SqlClient e criando o alias através da variável conn. Com isso podemos trabalhar com todas as funcionalidades da classe instanciada, sem termos que nos preocupar se os recursos serão liberados após sua utilização ou se a conexão será realmente encerrada.

Como isso acontece?

É importante lembrar que para a utilização do bloco using, necessita-se que a classe usada no escopo, implemente a interface IDisposable, ou seja, a classe deve implementar o método Dispose que está dentro das exigências da interface IDisposable, e neste método deve ser especificado como os recursos utilizados serão liberados na memória. Com isso, automaticamente no final do bloco using, o método Dispose é invocado pela CLR (Common Language Runtime).

Se formos nas definições da classe SqlConnection, iremos ver que a mesma herda de DbConnection que por sua vez implementa a interface IDisposable; por isso conseguimos utilizá-la no bloco using.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConexaoBD
{
    class UsuarioAplicacao
    {
        private BD bd;
        public void Insert(Usuarios usuario)
        {
            var strQuery = "";
            strQuery += "INSERT INTO usuarios(usuario_nome, usuario_cargo, usuario_data_registro)";
            strQuery += string.Format(" VALUES('{0}', '{1}', '{2}')",
                                     usuario.Nome, usuario.Cargo, usuario.Data);
            using (bd = new BD())
            {
                bd.ExecutaComando(strQuery);
            }
        }
        public void Update(Usuarios usuario)
        {
            var strQuery = "";
            strQuery += "UPDATE usuarios SET ";
            strQuery += string.Format("usuario_nome = '{0}', ", usuario.Nome);
            strQuery += string.Format("usuario_cargo = '{0}', ", usuario.Cargo);
            strQuery += string.Format("usuario_data_registro = '{0}' ", usuario.Data);
            strQuery += string.Format("WHERE usuario_id = '{0}';", usuario.Id);
            using (bd = new BD())
            {
                bd.ExecutaComando(strQuery);
            }
        }

    }
}
```

Fonte: DevMedia, MVP <http://www.devmedia.com.br/entendendo-o-bloco-using-no-c/16967>