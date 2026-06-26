# CSU — Landing Page (redesign) · Documento de entrega para a equipa de Dev

Plataforma do **Cadastro Social Único (CSU)**. Esta entrega substitui a landing
page anterior (que usava imagens de pessoas geradas por IA, não autorizadas pelo
Ministério) por um redesign **sem pessoas**, com identidade visual nas cores de
Angola (**preto, vermelho e amarelo**) e posicionamento de **sistema interno**
(acesso por login, não público/comercial).

---

## 1. Conteúdo do pacote

```
entrega-dev/
├── README.md                  ← este documento
├── CSU-Landing.html           ← versão AUTÓNOMA (abre offline, tudo embutido)
├── imagens/                   ← imagens originais (PNG/JPG)
│   ├── hero2masfamu.png            (hero — recolha noturna no terreno)
│   ├── Impactomasfamu.png          (impacto — comunidade rural + rede de dados)
│   ├── segurancamasfamu.png        (segurança — mapa de Angola + cadeado)
│   └── jornada-a-panorama.jpg      (ecrã de login — painel lateral)
└── fonte/                     ← código-fonte editável
    ├── CSU Landing B.dc.html       (página, com markup + estilos inline)
    ├── image-slot.js               (componente de imagem arrastável)
    ├── support.js                  (runtime do componente)
    └── uploads/                    (imagens referenciadas pela fonte)
```

> **Para visualizar agora:** abra `CSU-Landing.html` em qualquer browser. É um
> único ficheiro auto-contido (imagens e fontes embutidas), ideal para aprovação.

---

## 2. Identidade visual

### Cores
| Função | Hex |
|---|---|
| Vermelho institucional (ação) | `#C8102E` |
| Vermelho hover / escuro | `#A50C24` |
| Vermelho vivo (destaque de texto) | `#FF3B47` |
| Preto / fundo principal | `#16100E` · `#0B0807` |
| Amarelo (acento / realce) | `#F5B301` |
| Texto sobre claro | `#16100E` / `#3A322E` / `#6B635D` |
| Texto sobre escuro | `#FFFFFF` / `#C9BEB3` / `#B7ADA3` |
| Fundo claro alternativo | `#F7F3ED` |
| Linhas / bordas | `#E9E2D8` (claro) · `#2E2520` (escuro) |

As três cores de Angola são usadas com sobriedade: **preto** domina as zonas
fortes (hero, segurança, rodapé), **vermelho** é a cor de ação e da faixa de
métricas, **amarelo** é o acento (eyebrows, realces, passo final).

### Tipografia (Google Fonts)
- **Archivo** (700/800/900) — títulos, em CAIXA ALTA, peso forte.
- **IBM Plex Sans** (400/500/600/700) — corpo de texto e UI.

```html
<link href="https://fonts.googleapis.com/css2?family=Archivo:wght@600;700;800;900&family=IBM+Plex+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
```

### Outros tokens
- Raio dos cartões/botões: `8–14px`
- Largura máxima do conteúdo: `1240px`, padding lateral `30px`
- Faixa de cor no topo: barra vermelha (42%) + barra amarela (16%)

---

## 3. Estrutura das secções

1. **Cabeçalho** — fixo (sticky), fundo preto. Logo CSU + navegação + botão
   **Entrar** (abre o ecrã de login).
2. **Hero** — imagem de fundo inteira (deslocada à direita, esquerda preta com
   gradiente). Título "Proteger quem mais precisa de nós" + botão *Aceder ao sistema*.
3. **Métricas** — faixa vermelha com 4 indicadores e contagem animada ao entrar
   no ecrã: **1 270 000+** agregados · **21** províncias · **69** indicadores · **99%**.
4. **Funcionalidades** — lista editorial numerada (01–06) com divisórias.
5. **Como funciona** — linha do tempo de 4 passos (Recolha → Validação →
   Pontuação → Elegibilidade).
6. **Impacto social** — texto + imagem da comunidade rural.
7. **Segurança & privacidade** — imagem de fundo inteira (mapa de Angola, à
   direita) com a lista de garantias sobreposta à esquerda.
8. **CTA** — faixa amarela, "Entrar no sistema".
9. **Rodapé** — preto, navegação + legal.
10. **Ecrã de login** — overlay em ecrã inteiro (`side-by-side`): imagem à
    esquerda, formulário à direita com arte preto/vermelho/amarelo.

---

## 4. Comportamentos / JavaScript

- **Contagem animada das métricas** — anima de 0 até ao valor quando a faixa
  entra no viewport (`IntersectionObserver`).
- **Login** — o botão *Entrar* / *Aceder ao sistema* abre o overlay; o ✕ fecha.
  ⚠️ O formulário é **apenas maqueta de front-end** — submeter apenas fecha o
  overlay. **Falta integrar a autenticação real** (ver secção 6).
- **Imagens arrastáveis** (`<image-slot>`) — no ambiente de origem permitem
  arrastar/soltar uma imagem que fica gravada. No site final, os devs devem
  substituir por `<img>` normais (ver secção 6).

---

## 5. Dados / conteúdo (confirmar antes de publicar)

- **21 províncias** (Angola — divisão administrativa de 2024).
- Os restantes números das métricas (**1 270 000+**, **69**, **99%**) são
  **ilustrativos** — confirmar valores oficiais.
- E-mail/links do rodapé e CTA são placeholders.

---

## 6. Notas de implementação para os devs

A fonte está escrita como **HTML com estilos *inline*** (sem framework
obrigatório) — fácil de transpor para o stack existente (HTML/CSS/JS ou
React/Next). Recomendações:

1. **Imagens:** trocar os elementos `<image-slot ...>` por `<img src="...">`
   normais apontando para `imagens/`. Manter `object-fit: cover` e o
   posicionamento (à direita, com gradiente preto à esquerda) no hero e na
   secção de Segurança. Otimizar para `.webp`.
2. **Login real:** ligar o formulário ao serviço de autenticação interno
   (utilizador/email + palavra-passe), com proteção CSRF, *rate limiting* e
   sessão segura. O texto "Acesso restrito a utilizadores autorizados" deve
   manter-se.
3. **Acessibilidade:** já usa marcação semântica, contraste forte e respeita
   `prefers-reduced-motion` (desligar a animação das métricas nesse caso).
4. **Responsivo:** o layout foi desenhado para desktop. Adaptar os *grids* de
   3 e 4 colunas (Funcionalidades, Métricas, Passos) para 1–2 colunas em mobile,
   e empilhar o login (imagem em cima, formulário em baixo) abaixo de ~860px.
5. **Fontes:** carregar Archivo + IBM Plex Sans (Google Fonts ou *self-hosted*).

---

## 7. Versões

- **Direção B** (este pacote) — editorial/ousada, hero noturno de fundo inteiro.
  Foi a direção aprovada.
- Existe também uma *Direção A* (institucional sóbria, com serifa) no projeto,
  caso seja útil para comparação.

---

© 2026 Cadastro Social Único · Uso interno do sistema de proteção social.
