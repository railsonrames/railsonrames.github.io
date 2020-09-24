---
layout: post
title: 'Filtrando Coleções com o Entity Framework adicionalmente pelo contexto'
date: 2020-09-24 18:18:45
image: 'http://uploaddeimagens.com.br/images/001/603/663/full/artigo-dotnet-framework.png?1536495814'
description:
category: 'Code'
tags:
  - C#
  - Entity Framework
twitter_text:
introduction:
---

# Como filtrar um retorno de uma consulta com Entity Framework

Com o objeto recuperado como em:

```
var productObject = context
  .Products
  .Where(x => x.Id == 123)
  .FistOrDefault();
```

De posse do objeto eu quero acessar uma coleção interna dele onde eu tenho uma outra condição que é expressada dentro de outro método `Where`, fazendo da seguinte forma:

```
context.Entry(productObject)
  .Collection(x => x.Orders)
  .Query()
  .Where(x => x.Price > 10)
  .Load();
```

Assim teremos uma lista de produtos, cujo os quais foram originados de compras nas quais os valores dos produtos são superiores a 10 dinheiros.
