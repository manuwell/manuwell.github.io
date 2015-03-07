---
layout: post
title: "Importando certificado SSL no Ubuntu"
date: 2015-03-07 18:37:29 -0300
comments: true
categories: [linux, ssl]
---

Fala galera, blza?! Hoje vou compartilhar com vocês um problema
que passamos com certificados SSL em nosso ambiente de QA.

## Cenário

Conversa entre duas aplicações server-side com certificado SSL gerado via comando
openssl, que chamarei neste post de certificado auto-gerado.

<!-- more -->

## O problema

Quando a aplicação tentava conversar com nossa aplicação interna de OAuth via HTTPS,
com um certificado auto-gerado ela crashava.

Analisando os logs da aplicação que fazia o request, vímos uma mensagem de erro
de certificado SSL inválido.

Lógico que o certificado SSL era inválido, pois estávamos usando um que havíamos
gerado para nosso provedor de oauth, `oauth.provider.com`. Não eramos uma `CA` válida.
Então, como resolver esse problema?

## Solução

Pegamos o certificado gerado pelo comando:

```bash

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout oauth.provider.com.key
-out oauth.provider.com.crt
```

e, na máquina da aplicação que fazia o request `HTTPS` para o servidor de OAuth, importamos
o `.crt` dentro do `Sistema Operacional`.

Os certificados que o `SO` acredita serem válidos e de entidades certificadoras, as `CA`s,
ficam no diretório `/usr/share/ca-certificates`.

Todo request `HTTPS` no processo de validação de certificado olha para esse diretório.
Se o nosso certificado estivesse ali; batata! resolvido o problema.

Jogamos nosso certificado dentro desse diretório e avisamos o `SO` para recarregar
os certificados:

```bash

# jogando o certificado do provedor de autenticação no dir de certificados
sudo cp oauth.provider.com.crt /usr/share/ca-certificates/

# adicionando novo certificado na listagem
sudo echo oauth.provider.com >> /etc/ca-certificates.conf

# atualizando a lista de certificados existentes
sudo update-ca-certificates -f
```

Quando a aplicação realizou o request HTTPS ao nosso OAuth provider com o certificado importado.
Tudo fluiu perfeitamente bem.

Resolvemos um problema de comunicação no nível do `SO`. Sem precisar alterar uma linha de código da aplicação.

Sugestões como alterar o código para ignorar o certificado foram levantadas mas que bom,
que essa resolveu. Afinal, esse era um problema de ambiente e não queríamos comprar um certificado SSL
somente para o ambiente de `QA`.

É isso! E simbora!

## Referências

[http://man.he.net/man8/update-ca-certificates](http://man.he.net/man8/update-ca-certificates)
