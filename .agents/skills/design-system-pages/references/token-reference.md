# Token Reference — `<PREFIX>_TOKENS`

Referência da **forma canônica** do objeto de tokens do padrão
design-system-pages. Valores concretos variam por projeto; este documento
descreve os grupos de chaves que toda direção de token deve expor, e as
regras de consumo que mantêm telas funcionando em todas as direções.

## Estrutura

Um único objeto exportado via `Object.assign(window, { <PREFIX>_TOKENS })`,
com direções nomeadas (o padrão do formato são **duas**):

```js
const <PREFIX>_TOKENS = {
  A: { name: '...', tagline: '...', /* key set completo */ },
  B: { name: '...', tagline: '...', /* key set completo */ },
};
```

- Cada direção carrega o **mesmo conjunto de chaves** — é isso que permite um
  componente `{ t }` renderizar em qualquer direção.
- `name` e `tagline` descrevem a identidade (ex.: um claro "papel quente", um
  escuro "estúdio noturno").

## Grupos de chaves por direção

| Grupo | Chaves | Uso |
|---|---|---|
| Superfícies | `paper`, `surface`, `panel`, `sunken`, `line`, `lineStrong` | fundo da página, cartões, painéis, áreas recuadas, bordas |
| Tinta | `ink`, `ink2`, `muted`, `faint` | texto principal → hierarquia decrescente |
| Accent | `accent`, `accentHi`, `accentLo`, `accentInk` | ação primária, hover/press, tinta sobre accent |
| Status | `ok`, `okBg`, `warn`, `warnBg`, `err`, `errBg`, `info`, `infoBg` | badges e banners de status |
| Sombras | `shadowSm`, `shadowMd`, `shadowLg` | elevação em 3 níveis |

## Formato dos valores

- Cores preferencialmente em `oklch(L C H)` (L 0–1, C croma, H matiz) — facilita
  derivar variantes de uma mesma matiz e gerar direções claras/escuras com o
  mesmo DNA. Hex literal é aceitável para valores de marca pontuais.
- Um padrão comum: o HTML reconstrói as direções sobrescrevendo as chaves
  `accent*` com o matiz escolhido no painel de Tweaks (ver SKILL.md,
  convenção 6), permitindo explorar matizes sem editar os arquivos de token.

## Tipografia e forma

- Uma família para UI/display + uma mono para timestamps, metadados e labels
  de etapa é a combinação padrão (carregadas via Google Fonts no HTML).
- Escolha tipográfica e forma (grade, raio, sombras) seguem o assunto do
  produto — o formato referencia, não prescreve: mantenha consistência
  interna e registre a escala no artboard de referência de tokens.

## Regras de consumo

1. Componentes recebem `t` como prop e leem **somente** de `t` — nenhum valor
   de cor literal no JSX quando a chave existe em `t`.
2. Componentes nunca leem `<PREFIX>_TOKENS.A` ou `.B` diretamente; a direção
   é escolhida por quem compõe (o `App` no HTML).
3. Novas chaves são adicionadas **em todas as direções** ao mesmo tempo e,
   se afetarem o accent, também no bloco de reconstrução do HTML (quando
   existir).
4. O prefixo `<PREFIX>` é do projeto (ver regra de prefixo no SKILL.md):
   descubra o existente antes de nomear qualquer coisa nova.
