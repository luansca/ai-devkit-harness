# Design System Pages

Skill genérico e autossuficiente para criar e estender **páginas de design
system em código**: um único HTML autocontido que renderiza as telas de um
produto como mockups React (React UMD + Babel standalone, sem build) sobre um
canvas com pan/zoom estilo Figma, dirigido por design tokens in-code.

## Quando usar

- "Crie a página de design system / identidade visual deste produto"
- "Adicione o mockup da tela X à página de design"
- "Evolua os tokens ou os componentes primitivos do design"
- "Verifique se a implementação bate com o mockup"

## Uso rápido (visualizar uma página existente)

```bash
# a página precisa de rede para os scripts de CDN e fontes
cd <pasta-da-pagina>
python3 -m http.server 8123
# abrir http://localhost:8123/<pagina>.html
```

O SKILL.md carrega a anatomia completa do padrão, a regra de prefixo
(`<PREFIX>_TOKENS`, `<PREFIX>Button`...), as convenções obrigatórias e o passo
a passo tanto para **adicionar telas** a uma página existente quanto para
**bootstrapar** uma página nova do zero — agnóstico de projeto, produto e
stack de implementação.

## Qualidade

Se o projeto tiver rubrica de design ou agente avaliador, ela é a régua de
aceitação; sem rubrica, o padrão é identidade distinta e coesa específica do
domínio do produto — nunca estética de template genérico.
