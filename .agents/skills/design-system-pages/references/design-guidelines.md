# Design Guidelines — microcopy, typography, motion, critique

Guias de profundidade para o padrão design-system-pages. O SKILL.md carrega
o essencial; este arquivo detalha o *como*. Aplica-se a qualquer projeto — os
exemplos são ilustrativos, não prescritivos.

## Microcopy

Palavras aparecem num design para torná-lo mais fácil de entender e usar:
são conteúdo de design, não decoração. Antes de escrever qualquer texto,
pergunte o que a tela precisa comunicar e a melhor forma de dizê-lo para
quem navega pela experiência.

- **Escreva da perspectiva de quem usa.** Nomeie as coisas pela linguagem
  que o usuário entende, não pela arquitetura do sistema: "notificações",
  não "configuração de webhooks". Descreva o que algo faz em termos simples;
  ser claro e específico vence ser esperto.
- **Voz ativa e CTAs literais.** O botão diz exatamente o que acontece ao
  usá-lo: "Salvar alterações", nunca "Enviar". Uma ação mantém o mesmo nome
  pelo fluxo inteiro — o botão "Publicar" produz o toast "Publicado". O
  vocabulário de uma interface é a sinalização de quem a percorre;
  consistência é como as pessoas aprendem o caminho.
- **Erro e vazio são direção, não clima.** Explique o que deu errado e como
  resolver, na voz da interface — erros não pedem desculpa nem são vagos.
  Uma tela vazia é um convite à ação: diga o que fazer primeiro e por quê
  vale a pena.
- **Tom conversacional:** verbos simples, sentence case, sem enchimento,
  tom alinhado à marca e à audiência. Cada elemento escrito faz exatamente
  um trabalho.
- **No mockup, o copy é parte do design.** Escreva o texto real da tela
  (estados carregado/erro/vazio incluídos) — copy genérica "Welcome to
  [App]" deixaria o design tão templated quanto o layout.

## Typography

A tipografia carrega a personalidade da página.

- **Uma ou duas famílias**, claramente distintas quando forem duas. Não
  precise de família separada para display e corpo — tratamento tipográfico
  (peso, largura, espaço) transforma a mesma família.
- **Escala de tipo explícita** no plano de design: display, body, data/mono,
  com pesos intencionais. Guia padrão de escala: *The Elements of Typographic
  Style*.
- **Comprimento de linha** abaixo de 80 caracteres; serifas podem um pouco
  mais, com entrelinha maior que sans no corpo.
- **Tratamentos tipográficos a evitar** (os tells mais comuns de página
  gerada):
  - acentar uma única palavra ou frase do headline (itálico/negrito/cor);
  - all-caps em labels sem função informacional;
  - labels tipográficas desnecessárias acima do conteúdo;
  - mono para *tudo* que é dado pequeno — reserve a mono para o que de fato
    é dado técnico (timestamps, IDs, metadados) e deixe a escolha dizer isso.
- **Tratamento como elemento ativo:** quando o tipo é headline ou elemento
  visual, o próprio tratamento participa do design — não é veículo neutro.

## Motion

- Movimento não disparado pelo usuário é raro e deliberado: serve para
  chamar atenção. **Um único momento orquestrado** por tela (uma sequência
  de carregamento, uma revelação) rende mais que efeitos espalhados.
- Motion que **responde à ação** (abrir, expandir, confirmar) é bem-vindo
  quando mostra o que mudou.
- Tells genéricos a evitar: fade-and-slide-up de entrada em cada seção;
  transição de hover em todo cartão.

## Self-critique protocol

- **Critique com screenshot durante a construção**, não só no fim — imagem
  vale mais que mil tokens. O SKILL.md define o ciclo: inspecionar em lote
  (desktop + as direções de token juntas), corrigir tudo num passe, no
  máximo um passe de confirmação, e parar de polir.
- **Enquadre o board inteiro em cada captura.** O canvas renderiza as
  direções lado a lado dentro de um mundo com `transform` (translate +
  scale); o viewport raramente mostra tudo no zoom default. Para uma
  crítica completa: reduza o zoom (controles do canvas ou, via script,
  ajuste o `transform` do elemento-mundo — ex. `translate3d(8px, 8px, 0)
  scale(0.24)` — guardando o valor anterior), capture o screenshot, e
  **restaure o transform** ao final. Uma imagem com as duas direções juntas
  revela desequilíbrio de contraste e peso que captures por seção escondem.
- **Procure especificamente:** hierarquia quebrada, decoração que não serve
  ao conteúdo, tells da calibration checklist que escaparam, contraste entre
  direções, elementos cortados no limite do artboard.
- **Memória do projeto:** registre em `scraps/` as direções tentadas e
  rejeitadas (com uma linha de porquê), para passes futuros não repetirem o
  que já falhou. Humano criativo tem memória — o projeto também deve ter.
- **Regra do acessório:** antes de entregar, olhe o conjunto e remova um
  elemento. Se nada pode ser removido, o design provavelmente está mais
  ornamentado do que deveria.
