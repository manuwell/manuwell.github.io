---
layout: post
title: "Organizando assets de javascript no rails"
date: 2015-02-06 13:38:18 -0200
comments: true
categories:
  - rails
  - assets
---

Sabemos que o `application.js` centraliza os javascripts
de uma aplicação rails.

Este arquivo permitem que façamos includes de arquivos externos
e, de forma modularizada, compor nosso javascript da aplicação.

O que em nenhum lugar se fala ou comenta é: **como fazer scripts que rodem
em páginas específicas? Como estruturar de forma escalável seus assets?**

## Como estruturar então?

Em projetos que estou trabalhando começamos a usar uma `lib js` chamada
[page.js](https://github.com/lucasmazza/page.js) do [lucasmazza](https://github.com/lucasmazza).
Ela nos permite direcionar javascripts para actions de controllers específicas.

Esse é o primeiro passo para estruturar sua aplicação. Adicionar no
`vendor/assets/javascripts/` o arquivo `page.min.js`.

Tendo adicionado ao vendor, precisamos incluir no nosso `application.js`:

``` javascript
//= require page.min

$(document).ready(function(){
  // runs page js
  page.dispatch();
})
```

Configurado nosso `application.js`, bora identificar nossas actions.
Abra seu template do rails, `views/layouts/application.html.erb`

``` html
<body data-page="<%= controller_path %>#<%= action_name %>">
```

## Estrutura de diretórios

Agora já temos tudo preparado para escalar nossos scripts por página.

Criemos um diretório em:

``` bash
app/assets/javascripts/pages/
```

E criemos nosso primeiro script que roda somente para a rota "tweets#index":

``` javascript
// pages/tweets.js

page.at("tweets#index", function(){
  alert("showing my tweets");
})

page.at("tweets#show", function(){
  alert("showing one tweet");
})
```

E voilá.

Agora temos tudo uma estrutura escalável, de fácil manutenção e isolada de scripts.

Foi um grande aprendizado para mim que, no início, jogava meus scripts de forma
desornenada dentro do `application.js` ou em `arquivos` desconexos que incluía de forma
alentária nas páginas.

Espero ter ajudado. ;-)

