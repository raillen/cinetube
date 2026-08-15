# Local Recommendation Engine

## Objetivo

Recomendar mídia sem IA, embeddings, tracking remoto ou perfil cloud. O algoritmo deve ser rápido, explicável e reproduzível.

## Sinais por perfil

Valores iniciais sugeridos, calibráveis por testes:

- favorito: +5
- rewatch: +4
- concluído: +3
- assistiu >=70%: +2
- watch later: +1
- abandono <=15%: -2
- não tenho interesse: -6

## Features

Gêneros, keywords/subgêneros, pessoas, país, idioma, coleção/franquia e qualidade/nota externa. Aplicar decay temporal suave para interesses, sem apagar preferências históricas.

## Score inicial

`0.30*genre + 0.25*keyword + 0.15*people + 0.05*country + 0.05*language + 0.10*quality + 0.10*novelty`.

Penalizar já assistidos quando a intenção é descoberta, itens ocultos, excesso de repetição e disponibilidade inexistente.

## Regras

- Cálculo no Go/DB local.
- Nenhum evento de gosto precisa ser enviado a provider.
- A Home pode explicar: “Porque você assiste ficção científica”.
- Resultados devem ser diversificados para evitar uma única franquia dominar.
- Pesos futuros só mudam com benchmark/UX evidence; registrar versão do algoritmo.
