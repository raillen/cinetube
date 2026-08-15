# SuperFlix Integration

## Papel inicial

Adapter de catálogo/disponibilidade e playback, não modelo de domínio nem storage de usuário.

## Endpoints documentados considerados

- `/lista` para listagem/pesquisa em JSON e IDs TMDB/IMDb.
- `/filme/{id}` para player de filme.
- `/serie/{id}` e `/serie/{id}/{temporada}/{episodio}` para série/anime/dorama.
- parâmetros/fragmentos documentados do player podem ser encapsulados pelo adapter.

## Regras

- `baseURL` configurável e centralizada.
- Nunca espalhar URLs SuperFlix no domínio/UI.
- Resolver external IDs no adapter.
- Não depender de estrutura HTML privada.
- Não extrair `.m3u8`, DASH ou outras URLs não documentadas.
- Player remoto deve rodar em superfície isolada sem privileged Wails bindings.
- Timeout, retry limitado, cancellation, cache apenas quando permitido e health checks suaves.

## Falha

Retornar erro normalizado; preservar biblioteca local; permitir outro PlaybackProvider no futuro.

## Compliance

A integração deve obedecer termos vigentes do provider e requisitos legais aplicáveis antes de distribuição pública/comercial.
