---
title: "Roteiro para o WebApi e o ASP.NET MVC"
layout: post
image: 'http://www.ryadel.com/wp-content/uploads/2016/02/logo-aspnetmvc.png'
category: 'Code'
tags:
- C#
- ASP.NET MVC
- WebApi
date: 2017-10-09 01:30:00 -0300
---

Roteiro

* Criar projeto Web API
<br>	Web API
* Pasta Models
<br>	Funcionario (id, nome, funcao, turno, horastrabalhadas, valordahora)
* NuGet EntityFramework
* Criar pasta DAO
<br>	FuncionarioContext
<br>		DbSet<Funcionario>
* Criar novo banco em App_Data
<br>	nome.mdf
* Adicionar connectionStrings
<connectionStrings>

	<add name="FuncionarioContext" providerName="System.Data.SqlClient"
 connectionString="Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|Funcionario.mdf; Integrated Security=True"/>

</connectionStrings>
PM> Enable-Migrations
PM> Add-Migration CriacaoDoBanco
PM> Update-Database
- Criar FuncionarioController
	Template: Web API 2, using EF

- Criar projeto ASP.NET MVC
	MVC
- Pasta Models
	Funcionario (id, nome, funcao, turno, horastrabalhadas, valordahora, salariofinal, valorvenda)
	double getsalariofinal() > this.Funcao.Equals("Vendedor") > valordahora > 0 > vv*0.20
	elseif > faxineiro&&turno >  sf+=sf*0.05 > else sf > sf
	double getvalorvenda() > vv=200 > else > 0 > return
- Pasta Controllers
	Criar FuncionarioController
		IList = List
		client=WebClient > client.encoding=SysTEUTF8 > retornoJson=client.dowstring(@"api")
		list<dyn>list=newtons.json.jsonconv.deseObj<List<dyn>>(retornoJson.tostr();
		foreach(varFinList)
		new funcionario() > jogo de preenchimento func.Id=f.Id;
		func.vv=func.getvv(); func.sf=func.getsf();
		IList->funcionarios.Add(func);
	Retorna o próprio IList como elemento Model na view;
- Add view
	@model Ienumerable<pacote com o modelo>
	razor > htmlhelper .displaynamefor
	razor > foreach(variteminModel) > htmlhelper .displayfor(modelItem => item.Nome)
- Populara tabela com o exemplo
