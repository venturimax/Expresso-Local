# EXPRESSO LOCAL — Design System

## Status

- Direção visual aprovada: **Editorial Atlântico refinado pela Duna**.
- Este documento é a fonte de verdade visual para as sete páginas do site.
- O site existente é a base. O objetivo é refiná-lo, não substituí-lo por uma linguagem mais neutra.
- Alterações de interface devem preservar a autoridade visual, a escala tipográfica e o ritmo editorial já alcançados.

## 1. Tese visual

A EXPRESSO LOCAL combina a energia imediata do surf com a proximidade de uma escola local. O site deve parecer autoral, confiante e profissional, sem se tornar corporativo ou excessivamente polido.

O sistema reúne:

- a fotografia dominante, a assimetria e a escala expressiva da referência Hawaiian Skate;
- a clareza comercial, a organização das experiências e o fluxo de CTA da referência Surfing;
- superfícies mais calmas, cores menos saturadas e detalhes sofisticados da direção Duna Atlântica;
- elementos reais da EXPRESSO LOCAL: logotipo oficial, sede e identidade ligada a Itapema.

O resultado deve transmitir **mar, movimento, presença e cultura local**.

## 2. Princípios

### 2.1 Impacto antes de ornamento

O hero, as fotografias e os títulos de grande escala criam a primeira impressão. Detalhes decorativos nunca devem competir com esses elementos.

### 2.2 Calma não significa ausência de energia

Fundos neutros e cores dessaturadas organizam a leitura. A energia permanece na fotografia, na escala tipográfica, no movimento e nos contrastes pontuais.

### 2.3 Conteúdo real gera confiança

Fotografias reais da sede e da equipe têm prioridade. Imagens temporárias ou geradas devem ser identificadas como ilustrativas quando puderem ser confundidas com registros da escola.

### 2.4 Poucas decisões, tomadas com firmeza

Cada seção deve ter uma função clara. Evitar excesso de cards, selos, gradientes, sombras, cores e chamadas concorrentes.

### 2.5 Mobile é uma composição própria

No mobile, a hierarquia é reorganizada — não apenas reduzida. Títulos, imagens, navegação e CTAs devem continuar expressivos e fáceis de usar.

## 3. O que preservar da versão atual

- Hero fotográfico em tela cheia ou quase cheia.
- Título EXPRESSO LOCAL em escala dominante.
- Contraste forte entre fotografia, texto e CTA.
- Navegação comercial clara.
- Etiquetas numeradas e microtextos editoriais.
- Grandes áreas de respiro entre blocos.
- Alternância entre seções claras, seções coloridas e imagens de largura total.
- Ticker, tabs de experiências, FAQ e rodapé tipográfico.
- Estrutura de sete páginas: Início, Aulas, Aluguel, Loja, A escola, O mar e Dicas & bases.

## 4. O que refinar

- Substituir a marca tipográfica improvisada pelo logotipo oficial sempre que sua legibilidade permitir.
- Trocar Arial e Georgia pelo sistema Manrope + Instrument Serif.
- Suavizar fundos brancos e blocos azul-claros sem reduzir contraste.
- Reduzir a presença contínua do vermelho; usar terracota como acento editorial.
- Consolidar valores de cor, tipografia, espaçamento e movimento em tokens.
- Uniformizar botões, cards, formulários, tabs e estados interativos entre páginas.
- Integrar a fotografia real da sede na narrativa institucional.
- Refinar animações existentes e eliminar movimento decorativo desnecessário.
- Simplificar a navegação em larguras intermediárias e no mobile.

## 5. Cores

### 5.1 Tokens primitivos

| Token | Valor | Uso de origem |
|---|---:|---|
| `--color-foam-50` | `#FBFAF6` | Superfície clara elevada |
| `--color-foam-100` | `#F4F0E7` | Fundo principal |
| `--color-sand-200` | `#E7DFD2` | Fundo secundário e divisões suaves |
| `--color-line-300` | `#CEC8BB` | Bordas e linhas editoriais |
| `--color-mist-100` | `#DCE7E3` | Seções informativas claras |
| `--color-sea-500` | `#527A7E` | Azul-petróleo de apoio |
| `--color-sea-700` | `#315B61` | Cor principal de interface |
| `--color-sea-900` | `#193A3E` | Fundos escuros e hover |
| `--color-clay-500` | `#B86857` | Acento terracota |
| `--color-clay-700` | `#8E4337` | Acento com contraste maior |
| `--color-sun-500` | `#C3A35E` | Detalhe ocre raro |
| `--color-ink-900` | `#17211E` | Texto principal e fundo máximo |
| `--color-white` | `#FFFFFF` | Texto sobre superfícies escuras |

As cores vivas do logotipo oficial permanecem intactas dentro do próprio ativo. O restante da interface usa equivalentes mais controlados.

### 5.2 Tokens semânticos

| Token | Referência |
|---|---|
| `--color-background` | `var(--color-foam-100)` |
| `--color-surface` | `var(--color-foam-50)` |
| `--color-surface-muted` | `var(--color-mist-100)` |
| `--color-foreground` | `var(--color-ink-900)` |
| `--color-primary` | `var(--color-sea-700)` |
| `--color-primary-hover` | `var(--color-sea-900)` |
| `--color-accent` | `var(--color-clay-500)` |
| `--color-accent-strong` | `var(--color-clay-700)` |
| `--color-highlight` | `var(--color-sun-500)` |
| `--color-border` | `var(--color-line-300)` |
| `--color-on-dark` | `var(--color-foam-50)` |
| `--color-muted-text` | `#5C6964` |

### 5.3 Regras cromáticas

- Azul-petróleo é a principal cor funcional: botões, links ativos, seções escuras e foco visual.
- Terracota destaca palavras, números, indicadores e momentos editoriais. Não dominar páginas inteiras.
- Ocre aparece em pequenos detalhes da marca, nunca como fundo extenso ou texto de leitura.
- Fundos marfim substituem branco puro em áreas amplas.
- Vermelho vivo fica restrito ao logotipo e a casos em que a identidade oficial precise aparecer sem alteração.
- Todo par de texto e fundo deve manter contraste acessível. Elementos decorativos não podem ser a única forma de comunicar estado.

## 6. Tipografia

### 6.1 Famílias

- **Manrope** — títulos sans, corpo, navegação, botões, formulários, números e metadados.
- **Instrument Serif** — palavras de contraste, frases editoriais e destaques em itálico.
- Fallback sans: `Arial, Helvetica, sans-serif`.
- Fallback serif: `Georgia, "Times New Roman", serif`.

Usar no máximo duas famílias por composição. Instrument Serif nunca deve substituir textos longos ou informações funcionais.

### 6.2 Pesos

- Manrope 400: corpo.
- Manrope 500: corpo de destaque e labels.
- Manrope 600: navegação e controles.
- Manrope 700: títulos intermediários e botões.
- Manrope 800: displays e grandes chamadas.
- Instrument Serif 400: contrastes editoriais.

### 6.3 Escala fluida

| Papel | Tamanho recomendado | Entrelinha |
|---|---|---|
| Hero display | `clamp(5rem, 15.7vw, 14.5rem)` | `0.82–0.90` |
| Display de seção | `clamp(3rem, 6.5vw, 7rem)` | `0.94–1.02` |
| Título editorial | `clamp(2.4rem, 4.5vw, 4.5rem)` | `1.00–1.08` |
| Título de card | `clamp(1.35rem, 2vw, 2rem)` | `1.08–1.18` |
| Lead | `clamp(1.1rem, 1.5vw, 1.4rem)` | `1.45–1.60` |
| Corpo | `1rem` | `1.60–1.75` |
| Navegação | `0.875rem` | `1.2` |
| Eyebrow e metadata | `0.75rem` | `1.3` |

### 6.4 Composição

- Displays usam tracking negativo entre `-0.075em` e `-0.04em`.
- Texto corrente usa tracking normal.
- Microtextos podem usar caixa alta e tracking entre `0.08em` e `0.16em`.
- Quebras de título devem ser editoriais e intencionais. Nunca deixar palavras isoladas por acaso.
- No mobile, reduzir escala e recompor quebras; não comprimir títulos até perder presença.

## 7. Layout e espaçamento

### 7.1 Grid

- Desktop: grid editorial de 12 colunas.
- Tablet: 8 colunas.
- Mobile: 4 colunas.
- Largura máxima de conteúdo: `1440px`.
- Margens laterais desktop: `4.5%–6.5%`.
- Margens laterais mobile: `6%` ou mínimo de `20px`.

### 7.2 Escala de espaçamento

Base de 4px:

`4, 8, 12, 16, 24, 32, 48, 64, 80, 96, 128`.

- Espaçamento interno de controles: 12–20px.
- Espaçamento interno de cards: 24–40px.
- Separação entre blocos relacionados: 24–48px.
- Seções desktop: 88–128px de padding vertical.
- Seções mobile: 56–80px de padding vertical.

### 7.3 Bordas, raios e sombras

- Linhas de 1px são o principal recurso de separação.
- Botões principais usam raio completo.
- Cards editoriais usam raio entre 0 e 8px; não arredondar tudo.
- Fotos podem usar raio discreto de até 8px quando não forem full bleed.
- Sombras são sutis e reservadas a elementos elevados ou interativos.
- Evitar glassmorphism, sombras coloridas e gradientes decorativos sem função.

## 8. Fotografia

### 8.1 Hierarquia de ativos

1. Fotografias reais da EXPRESSO LOCAL.
2. Fotografias reais de aulas, equipe e ambiente autorizadas pelo cliente.
3. Imagens ilustrativas temporárias claramente identificadas quando necessário.

### 8.2 Tratamento

- Manter cores naturais e sensação de sol real.
- Saturação pode ser reduzida levemente, nunca a ponto de parecer cinza ou artificial.
- Contraste deve preservar detalhes de água, céu, pele e equipamentos.
- Usar overlays apenas para garantir legibilidade de texto.
- Cortes devem preservar o sujeito e deixar áreas de respiro para títulos.
- Evitar repetir a mesma fotografia em vários blocos de uma página final.

### 8.3 Uso dos ativos fornecidos

- `expresso.jpeg`: logotipo oficial e referência cromática. Preparar versão otimizada e, se possível, fundo transparente sem alterar a composição.
- `frente.jpeg`: fotografia real da sede. Usar prioritariamente na página A escola ou em seção institucional da home.
- Não usar o texto “desde 2012” como afirmação histórica fora do logotipo sem confirmação do cliente.

## 9. Componentes

### 9.1 Cabeçalho

- Desktop: logotipo, navegação principal e CTA destacado.
- Preservar clareza comercial da versão atual.
- Em largura intermediária, mover itens secundários para “Mais” antes de comprimir excessivamente.
- Mobile: logotipo, CTA curto quando houver espaço e menu acessível.
- Estado ativo por cor e sublinhado discreto.

### 9.2 Botões

**Primário**

- Fundo azul-petróleo.
- Texto marfim ou branco.
- Raio completo.
- Altura mínima de 48px.
- Hover escurece o fundo e desloca no máximo 2px.

**Secundário**

- Fundo transparente.
- Borda de 1px.
- Hover com preenchimento leve.

**Link editorial**

- Texto com sublinhado fino.
- Mudança de cor no hover.
- Sem setas decorativas obrigatórias.

### 9.3 Labels editoriais

- Numeração, nome de seção e contexto em caixa alta.
- Fonte mínima de 12px.
- Linha horizontal pode separar label e conteúdo.
- Usar com consistência, não em todos os blocos internos.

### 9.4 Cards

- Hierarquia: eyebrow, título, descrição e ação.
- Fundo marfim ou transparente.
- Borda fina e acento cromático discreto.
- Hover pode alterar fundo, borda ou imagem; nunca todos ao mesmo tempo.
- Cards de produto ou aluguel não devem afirmar estoque, preço final ou disponibilidade sem dados confirmados.

### 9.5 Tabs de experiências

- Manter navegação por teclado e estados ARIA.
- Aba ativa indicada por texto, contraste e linha — não apenas cor.
- Mudanças de conteúdo usam transição curta, sem carrossel automático.

### 9.6 Formulários

- Labels visíveis, campos com no mínimo 48px e mensagens próximas ao campo.
- Foco com outline de alto contraste.
- Estado desabilitado precisa explicar por que a ação não está disponível.
- Pedidos são consultas. Não calcular preço final nem confirmar disponibilidade.
- Sem WhatsApp oficial configurado, oferecer instrução segura ou canal confirmado; nunca usar número fictício.

### 9.7 FAQ

- Accordion acessível com `<details>`/`<summary>` ou comportamento equivalente.
- Perguntas separadas por linhas finas.
- Ícone muda de estado com rotação curta.
- Texto de resposta com largura confortável para leitura.

### 9.8 Rodapé

- Fundo escuro.
- Marca em grande escala e navegação agrupada.
- Exibir apenas contatos e redes confirmados.
- Incluir localização geral Itapema / SC sem inventar endereço.

## 10. Movimento

O movimento deve lembrar fluxo e progressão, sem simular ondas literalmente.

| Interação | Duração | Curva |
|---|---:|---|
| Hover de controle | 160–220ms | `ease-out` |
| Entrada de texto/bloco | 500–750ms | `cubic-bezier(.22,1,.36,1)` |
| Troca de imagem/tab | 350–550ms | `ease` |
| Zoom fotográfico | 700–1200ms | `ease-out` |
| Ticker | 28–40s | linear |

### Regras

- Reveal vertical máximo de 24px.
- Stagger máximo de 80ms entre itens relacionados.
- Imagens podem ampliar entre 1.02 e 1.04 no hover.
- Não iniciar várias animações contínuas simultaneamente.
- Conteúdo permanece visível se JavaScript falhar.
- Com `prefers-reduced-motion: reduce`, remover animações, transições não essenciais, parallax e rolagem suave.

## 11. Arquitetura visual das páginas

### Início

- Hero fotográfico dominante.
- Manifesto curto.
- Experiências em tabs.
- Seção cultural/imagem de largura total.
- Passos ou princípios.
- CTA para consulta.
- FAQ.
- Caminhos para aluguel, loja e condições.
- Galeria ou Instagram apenas com fontes confirmadas.

### Aulas

- Hero editorial mais compacto.
- Comparação clara das experiências.
- O que esperar e como funciona.
- Formulário de consulta.
- Conteúdo de segurança apenas quando confirmado.

### Aluguel

- Categorias e equipamentos.
- Simulação sempre identificada como estimativa quando aplicável.
- Estoque e disponibilidade confirmados pela equipe.
- Formulário de consulta.

### Loja

- Vitrine organizada e filtros simples.
- Itens sem disponibilidade confirmada não parecem compráveis imediatamente.
- Consulta ou intenção de compra, não checkout fictício.

### A escola

- Fotografia real da sede como protagonista.
- História, equipe e credenciais somente quando confirmadas.
- Localização geral e contato confirmado.

### O mar

- Conteúdo educativo e links para fontes autorizadas.
- Não apresentar previsão copiada, desatualizada ou sem integração autorizada.
- Diferenciar informação geral de condição em tempo real.

### Dicas & bases

- Conteúdo editorial escaneável.
- Acordeões, listas ou artigos curtos.
- Navegação interna quando houver volume suficiente.

## 12. Responsividade

### Desktop

- Explorar escala, assimetria e imagens dominantes.
- Manter comprimentos de linha confortáveis.
- Evitar seções com conteúdo pequeno perdido em áreas grandes.

### Tablet

- Reduzir itens de navegação antes de diminuir fontes abaixo do recomendado.
- Grids de três colunas passam para duas.
- Preservar a força das imagens.

### Mobile

- Hero entre 75 e 92svh quando a imagem sustentar a composição.
- Títulos recompostos entre 11vw e 17vw conforme o conteúdo.
- Cards em uma coluna ou carrossel somente quando houver benefício real.
- Controles e links com área de toque mínima de 44×44px.
- Nenhum conteúdo pode causar rolagem horizontal.
- CTA principal deve aparecer cedo sem cobrir o conteúdo.

## 13. Acessibilidade

- Contraste mínimo conforme WCAG AA para textos e controles.
- Texto principal com pelo menos 16px.
- Navegação completa por teclado.
- Foco sempre visível.
- Hierarquia correta de headings.
- Texto alternativo descreve função e conteúdo da imagem.
- Imagens decorativas usam `alt=""`.
- Tabs, menu, filtros, carousel e accordions anunciam estado corretamente.
- Conteúdo não depende apenas de cor, posição ou animação.
- Zoom de texto a 200% sem perda de conteúdo ou funcionalidade.

## 14. Linguagem e conteúdo

- Tom próximo, seguro e direto.
- Evitar promessas exageradas, jargão de marketing e slogans repetidos.
- Não inventar preços, horários, duração, equipamentos incluídos, disponibilidade, endereço, telefone, credenciais, depoimentos ou regras operacionais.
- Informações não confirmadas devem ser omitidas ou tratadas como consulta.
- “Agendar” significa iniciar uma conversa; a confirmação é feita pela equipe.

## 15. Tokens de componente sugeridos

```css
:root {
  --button-primary-bg: var(--color-primary);
  --button-primary-bg-hover: var(--color-primary-hover);
  --button-primary-fg: var(--color-on-dark);
  --button-radius: 999px;
  --button-height: 48px;

  --card-bg: var(--color-surface);
  --card-border: var(--color-border);
  --card-radius: 4px;
  --card-padding: clamp(1.5rem, 3vw, 2.5rem);

  --input-bg: var(--color-surface);
  --input-border: var(--color-border);
  --input-focus: var(--color-primary);
  --input-height: 48px;

  --section-space: clamp(4rem, 8vw, 8rem);
  --content-gutter: clamp(1.25rem, 5vw, 5.5rem);

  --motion-fast: 180ms;
  --motion-normal: 500ms;
  --motion-slow: 750ms;
  --motion-ease-out: cubic-bezier(.22, 1, .36, 1);
}
```

## 16. Critérios de validação

Antes de considerar uma alteração visual concluída:

- comparar com a versão atual para confirmar que a autoridade visual não diminuiu;
- validar desktop em 1440×900;
- validar mobile em 375×812;
- testar navegação por teclado e foco;
- testar `prefers-reduced-motion`;
- verificar contraste de textos e controles;
- verificar ausência de rolagem horizontal;
- confirmar que formulários não prometem preço ou disponibilidade;
- conferir se fotografias reais e ilustrativas estão corretamente tratadas;
- revisar todas as sete páginas para consistência de tokens e componentes.

## 17. Anti-padrões

- Tornar todo o site bege, pálido ou sem contraste.
- Reduzir a escala do hero para parecer “mais clean”.
- Aplicar terracota, azul e ocre simultaneamente em todos os componentes.
- Usar serif em parágrafos longos ou controles.
- Adicionar badges, estrelas e selos apenas para preencher espaço.
- Arredondar todos os cards e imagens.
- Usar animação contínua em vários pontos da mesma viewport.
- Repetir a mesma fotografia como solução para falta de conteúdo.
- Apresentar placeholder como informação real da escola.
- Copiar o mockup Duna anterior como substituição da versão existente.

## 18. Decisão final

O norte visual não é “deixar tudo suave”. É **manter o impacto profissional da versão atual e elevar sua consistência**.

A Duna Atlântica atua em superfícies, tipografia, ritmo, detalhes e componentes. A energia permanece no hero, nas imagens, na escala e na personalidade EXPRESSO LOCAL.
