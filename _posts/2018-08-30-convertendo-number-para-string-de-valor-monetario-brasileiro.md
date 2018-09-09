---
layout: post
title: "Convertendo number para string de valor monetario brasileiro"
date: 2018-08-30 17:10:22
image: 'http://uploaddeimagens.com.br/images/001/603/651/full/artigo-conversao-monetaria.png?1536494741'
description: Uma mão na roda para o trabalho do dia a dia
category: 'Code'
tags:
- JavaScript
twitter_text:
introduction: Como formatar um valor number em Javascript para uma string no formato monetário do Brasil
---

# Convertendo um *number* para monetário e vice versa

Este post é voltado a uma utilidade quando se trata de converter valores *number* em Javascript, atualmente trabalho em um projeto com uso da linguagem C#, para o *front-end* se tem a disposição o *Razor*, mas muitas vezes converto os objetos que advem da *Model* para JSON e tratando diretamente no *script* do respectivo JS é preciso dar alguns tratamento para se visualizar da maneira desejada.

Separei esse tutorial em dois segmentos, vamos ao primeiro:

> Convertendo number para string R$, já aviso que achei estranho esse modo de fazer, mas irei sugerir o meu preferido no final.

```JS
function numeroParaMoeda(n, c, d, t){
    c = isNaN(c = Math.abs(c)) ? 2 : c, d = d == undefined ? "," : d, t = t == undefined ? "." : t, s = n < 0 ? "-" : "", i = parseInt(n = Math.abs(+n || 0).toFixed(c)) + "", j = (j = i.length) > 3 ? j % 3 : 0;
    return s + (j ? i.substr(0, j) + t : "") + i.substr(j).replace(/(\d{3})(?=\d)/g, "$1" + t) + (c ? d + Math.abs(n - i).toFixed(c).slice(2) : "");
}
```

> Para tanto precisamos de uma legenda pra corresponder a sopa de letras, logo:
* n -> é o número a converter
* c -> é a quantidade de casas decimais
* d -> é o elemento separador
* t -> é o elemento separador de **milhar**

Como dito anteriormente não é muito didático, mas cumpre o seu papel, olhe a entrada e respectiva saída:

```JS
var a = numeroParaMoeda(10);
var b = numeroParaMoeda(100);
var c = numeroParaMoeda(0.5);
var d = numeroParaMoeda(1500);
var e = numeroParaMoeda(89);
```

```JS
"10,00"
"100,00"
"0,50"
"1.500,00"
"89,00"
```
Agora o contrário, a.k.a o segundo:

```JS
function moedaParaNumero(valor)
{
    return isNaN(valor) == false ? parseFloat(valor) :   parseFloat(valor.replace("R$","").replace(".","").replace(",","."));
}
```
Muito mais tranquilo por conta do replace para desfazer, ponto positivo para esse método. Vamos as entradas e respectivas saídas:

```JS
var a = moedaParaNumero("R$ 10,00");
var b = moedaParaNumero("R$ 100");
var c = moedaParaNumero("0,50");
var d = moedaParaNumero("1.500,00");
var e = moedaParaNumero("89");
```

```JS
10
100
0.5
1500
89
```
Esses dois exemplos e os métodos foram extraídos de um excelente post no [Blog de Filipe Mansano](http://blog.fmansano.com/javascript/converter-numero-para-moeda-e-vice-versa/), entretanto eu li a documentação disponibilizada pela Mozilla em [MDN web docs](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString), adicionalmente a lista de códigos ISO para países, que pode ser visitada [no site Currency ISO.org](https://www.currency-iso.org/en/home/tables/table-a1.html), *a do Brazil é BRL*. Então, por último, mas não menos importante o método que utilizo e acredito ser a maneira mais tranquila:
```LS
function monetarioBrasil(valor) {
    return valor.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
};
```