# 📖 Contexto e Decisões de Arquitetura — Game Cards

Este documento descreve a visão do produto, as diretrizes de design, as decisões de engenharia de software e os critérios técnicos adotados no desenvolvimento do **Sistema de Cartas de Personagens de Jogos**.

---

## 1. Visão do Produto

O **Game Cards** é uma plataforma web full stack projetada para entusiastas de jogos eletrônicos e colecionismo digital. Diferente de um painel administrativo tradicional, a experiência foi concebida para evocar a imersão de álbuns de figurinhas e card games lendários, onde cada personagem possui:

- **Arte e Estética Própria**: Representada por ilustrações com placeholders gráficos estilizados em SVG vetorial.
- **Raridade**: Classificação em 6 níveis com identificação por cores, bordas e efeitos visuais luminosos (glow).
- **Atributos Canônicos**: Ano de lançamento original, jogo de estreia, preço em reais e descrição de história.
- **Efeito Misterioso (Blur)**: Imagens inicialmente desfocadas na galeria principal, revelando seus detalhes ao passar o mouse, tocar na tela ou focar via teclado.

---

## 2. Decisões de Arquitetura

### 2.1. Desacoplamento Frontend / Backend
O frontend e o backend residem em diretórios independentes (`frontend/` e `backend/`). Essa separação permite:
- **Deploy independente**: A API pode ser publicada como Serverless Functions na **Vercel**, enquanto o frontend pode ser hospedado em qualquer CDN ou provedor de arquivos estáticos (Vercel, GitHub Pages, Netlify, Cloudflare Pages).
- **Configuração de Domínio Dinâmica**: O serviço de API no frontend (`frontend/js/api.js`) lê a URL base a partir do `localStorage` ou de configurações globais, permitindo alternar entre o servidor local (`http://localhost:5000/api`) e o ambiente de produção na Vercel com um clique, sem modificar o código-fonte.

### 2.2. Conexão com MongoDB em Ambiente Serverless
Em plataformas serverless como a Vercel, cada execução pode instanciar um novo contêiner efêmero. Abrir uma nova conexão com o MongoDB a cada requisição esgotaria rapidamente o limite de conexões do MongoDB Atlas.
- **Solução Implementada**: Em `backend/src/config/database.js`, é implementado um padrão de **Connection Caching** no objeto global (`global.mongoose`). Conexões ativas são reutilizadas entre invocações da mesma instância Lambda/Serverless.

### 2.3. Frontend com Tecnologias Nativas (Vanilla JS)
Conforme especificação, nenhum framework frontend (React, Vue, Angular) foi utilizado:
- **HTML5 Semântico**: Uso de `<article>`, `<section>`, `<nav>`, `<main>`, `aria-labels` e tags semânticas para garantir total acessibilidade.
- **CSS3 Moderno**: CSS Custom Properties (variáveis de tema), Flexbox, CSS Grid, `filter: blur()`, backdrop-filter, animações suaves e design responsivo mobile-first.
- **JavaScript Puro**: Fetch API nativa para requisições assíncronas, manipulação direta e performática do DOM.

---

## 3. Modelo de Domínio e Validações

O modelo Mongoose `Character` encapsula o domínio da aplicação com validações rigorosas:

1. **`name`**: Obrigatório, string de 2 a 100 caracteres, trimmed.
2. **`game`**: Jogo de estreia original, obrigatório, 2 a 120 caracteres.
3. **`releaseDate`**: Data de lançamento original do jogo. Armazenada como objeto `Date` normalizado em UTC, evitando distorções causadas por fusos horários locais ao formatar apenas a data ou o ano.
4. **`imageUrl`**: Aceita URLs externas válidas (`http://...`, `https://...`) ou caminhos relativos locais (`assets/placeholders/...`). Inclui tratamento de fallback automático caso o arquivo ou link falhe.
5. **`rarity`**: Enum restrito às 6 opções oficiais:
   - **Comum**: Cinza/prata metálico.
   - **Incomum**: Verde esmeralda.
   - **Rara**: Azul safira elétrico.
   - **Épica**: Roxo ametista místico.
   - **Lendária**: Dourado solar cintilante.
   - **Mítica**: Vermelho rubi / gradiente holográfico cósmico.
6. **`price`**: Float com precisão de 2 casas decimais, maior ou igual a zero.
7. **`description`**: Texto livre explicativo, opcional (máx. 1000 caracteres).
8. **`createdAt` / `updatedAt`**: Gerenciados automaticamente pelos timestamps do Mongoose.

---

## 4. Política de Placeholders de Imagem

Em conformidade estrita com o requisito de **não utilizar inteligência artificial para geração de imagens**:
- Foram criados 5 arquivos vetoriais **SVG locais** para os personagens iniciais:
  - `connor-detroit.svg`: Temática cyberpunk, anel LED CyberLife e gradientes azuis.
  - `arthur-morgan-rdr2.svg`: Temática faroeste, silhueta montanhosa e estrela de pistoleiro.
  - `doom-slayer-doom.svg`: Temática infernal e industrial, visor Praetor e cor avermelhada.
  - `bayonetta.svg`: Temática gótica mágica, lua crescente, asas e tons de ametista.
  - `leon-resident-evil.svg`: Temática policial e perigo biológico do R.P.D.
- Um placeholder coringa `default-card.svg` é utilizado caso novas cartas sejam cadastradas sem arte própria ou ocorra falha de rede ao carregar URLs externas.

---

## 5. Experiência do Usuário (UX) e Acessibilidade

- **Tema Padrão**: Dark mode elegante e cinematográfico, com botão de alternância para tema claro memorizado no navegador via `localStorage`.
- **Efeito de Revelação (Blur Mystery)**:
  - Inicialmente, a imagem da carta possui `filter: blur(10px) brightness(0.65)`.
  - Ao passar o mouse, tocar (mobile) ou navegar via tecla `Tab`, a classe correspondente remove o filtro suavemente com `transition: filter 0.5s ease`, revelando a arte em alta definição e destacando os metadados.
  - O conteúdo textual (nome, jogo, ano e preço) permanece sempre no DOM com atributos acessíveis e sem filtro de blur, garantindo que usuários que utilizam leitores de tela tenham acesso total aos dados.
