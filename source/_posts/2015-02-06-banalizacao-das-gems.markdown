---
layout: post
title: "Banalização das Gems"
date: 2015-02-06 21:27:33 -0200
comments: true
categories:
  - rails
  - gems
---

Evite adicionar `gems` para funções específicas, ou que fazem
mais do que você necessita.

Vivemos uma era em que não faltam `gems` para as coisas
mais específicas possíveis. Temos `gems` para instalação de
bootstraps, deixar console colorido e por aí afora.

<!-- more -->

## Banalização das GEMS

Com `gems` cada vez mais específicas e sem um critério para sua
instalação na aplicação, vamos cada vez mais inflando nosso projeto
com dependências externas.

Essas `gems` podem possuir suporte, mas por vezes não conseguem
se manter atualizada ao longo da evolução da sua `aplicação`.

Quando houver a necessidade de atualização de seu rails por exemplo,
e uma dessas `gems` específicas não der suporte, vc. se encontrará numa
situação difícil. Sem tempo hábil para a remoção da `gem` que quebra
sua aplicação vc. provavelmente optará por deixar de lado
a atualização de seu `rails`.

## Conclusão

Tenha sempre o menor número de dependências em seu projeto. Abra os fontes
das `gems` que deseja instalar. Veja se a sua necessidade não é um trecho
de código que possa ser extraído em um `initializer`, `lib` dentro do rails.

Tenha sempre seu `Gemfile` o mais enxuto possível. Isso lhe facilitará em
manter sua aplicação atualizada e lhe gerará menos dor de cabeça no futuro.
