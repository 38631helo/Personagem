# 🎮 Game Cards — Sistema de Cartas de Personagens de Jogos

Uma aplicação web full stack moderna e imersiva para colecionar, gerenciar e exibir personagens lendários de videogames em formato de cartas colecionáveis.

O projeto conta com um **backend REST em Node.js com Express e MongoDB/Mongoose** preparado para deploy serverless na **Vercel**, e um **frontend desacoplado construído exclusivamente em HTML5, CSS3 e JavaScript puro**, consumindo os endpoints via Fetch API.

---

## 🌟 Destaques do Projeto

- 🎴 **Efeito Misterioso de Imagem com Blur**: As cartas aparecem inicialmente com arte misteriosa desfocada (`filter: blur()`). Ao passar o mouse, tocar no celular ou navegar via teclado, o desfoque dissipa suavemente revelando os detalhes do herói.
- 🌈 **Identidade Visual por Raridade**: Cores, bordas e auras luminosas exclusivas para 6 níveis: *Comum*, *Incomum*, *Rara*, *Épica*, *Lendária* e *Mítica*.
- 🎠 **Slider Horizontal Interativo**: Carrossel responsivo com suporte a setas, paginação por indicadores (dots), teclado (`ArrowLeft` / `ArrowRight`) e arrasto por toque (touch swipe) ou mouse.
- ⚡ **CRUD Completo com Persistência**: Cadastro, listagem, visualização detalhada, edição e exclusão de personagens com persistência definitiva no MongoDB.
- 🔎 **Busca, Filtros e Ordenação no Backend**: Pesquisa por texto no nome ou jogo, filtro por raridade e ordenação por preço e ano de lançamento original.
- 🌓 **Tema Escuro Padrão e Tema Claro**: Alternância rápida de tema com preferência salva no `localStorage`.
- 🖼️ **Placeholders Gráficos Locais em SVG**: Sem uso de imagens por inteligência artificial — vetores geométricos estilizados para os personagens iniciais e fallback automático caso uma imagem externa falhe.
- ☁️ **Serverless Ready (Vercel & MongoDB Atlas)**: Conexão Mongoose otimizada com cache para evitar estouro de pool de conexões em ambientes serverless.

---

## 🛠️ Tecnologias Utilizadas

### Backend
- **Node.js** (v18+)
- **Express.js** (API REST)
- **MongoDB** & **Mongoose** (Modelagem de dados e persistência)
- **CORS** (Configuração de origens cruzadas)
- **Dotenv** (Gerenciamento de variáveis de ambiente)
- **Jest** & **Supertest** (Testes automatizados de integração)
- **mongodb-memory-server** (Banco em memória para execução de testes sem dependência externa)

### Frontend
- **HTML5 Semântico**
- **CSS3 Moderno** (CSS Grid, Flexbox, Custom Properties, Transitions, Backdrop Filter)
- **JavaScript Puro (Vanilla JS)** (Fetch API, eventos de toque, manipulação do DOM sem frameworks)

---

## 📁 Estrutura de Pastas

```text
game-cards/
├── backend/
│   ├── src/
│   │   ├── config/
│   │   │   └── database.js          # Conexão Mongoose com cache para serverless
│   │   ├── controllers/
│   │   │   └── characterController.js # Lógica de negócio do CRUD e filtros
│   │   ├── models/
│   │   │   └── Character.js         # Schema Mongoose com validações e índices
│   │   ├── routes/
│   │   │   └── characterRoutes.js   # Definição das rotas REST
│   │   ├── middlewares/
│   │   │   └── errorHandler.js      # Tratamento centralizado de erros
│   │   ├── app.js                   # Instância do Express e middlewares
│   │   ├── server.js                # Inicializador do servidor HTTP local
│   │   └── seed.js                  # Script manual para popular dados iniciais
│   ├── tests/
│   │   └── character.test.js        # Testes de integração automatizados
│   ├── package.json
│   ├── vercel.json                  # Configuração para deploy serverless na Vercel
│   ├── .env                         # Variáveis de ambiente locais (ignorado no git)
│   └── .env.example                 # Modelo de variáveis de ambiente
│
├── frontend/
│   ├── index.html                   # Página inicial com slider interativo e blur
│   ├── characters.html              # Painel completo de gerenciamento (CRUD)
│   ├── css/
│   │   └── style.css                # Estilos globais, temas e efeitos visuais
│   ├── js/
│   │   ├── api.js                   # Cliente HTTP e utilitários de formatação
│   │   ├── slider.js                # Componente de slider responsivo com blur
│   │   └── characters.js            # Lógica da tela de gerenciamento e modais
│   └── assets/
│       └── placeholders/
│           ├── connor-detroit.svg
│           ├── arthur-morgan-rdr2.svg
│           ├── doom-slayer-doom.svg
│           ├── bayonetta.svg
│           ├── leon-resident-evil.svg
│           └── default-card.svg
│
├── Roadmap.md                       # Ciclo de vida e próximas iterações
├── Contexto.md                      # Decisões de arquitetura e regras de negócio
├── api.md                           # Documentação detalhada dos endpoints
├── .gitignore
└── README.md
```

---

## 🚀 Como Executar o Projeto Localmente

### 1. Pré-requisitos
- **Node.js** (versão 18 ou superior instalada).
- **MongoDB** (instância local rodando na porta 27017 ou cluster gratuito no **MongoDB Atlas**).

---

### 2. Configuração do Backend

1. Acesse o diretório do backend pelo terminal:
   ```bash
   cd backend
   ```

2. Instale as dependências:
   ```bash
   npm install
   ```

3. Crie e configure o arquivo `.env` a partir do modelo `.env.example`:
   ```bash
   # No Windows (PowerShell)
   Copy-Item .env.example .env

   # No Linux / macOS
   cp .env.example .env
   ```

4. Edite o `.env` caso deseje utilizar o MongoDB Atlas:
   ```env
   PORT=5000
   MONGODB_URI=mongodb://localhost:27017/game_cards
   CORS_ORIGIN=*
   NODE_ENV=development
   ```

5. **Execute o script de Seed** para cadastrar os personagens iniciais:
   ```bash
   npm run seed
   ```
   > ℹ️ **Nota**: O script de seed verifica se os personagens já existem e evita duplicá-los mesmo se for executado várias vezes seguidas.

6. Inicie o servidor da API:
   ```bash
   # Modo de desenvolvimento com autoreload (Nodemon)
   npm run dev

   # Ou modo produção
   npm start
   ```

   O backend estará acessível em: **`http://localhost:5000`**  
   Health check: **`http://localhost:5000/api/health`**  
   Endpoint de personagens: **`http://localhost:5000/api/characters`**

---

### 3. Execução dos Testes Automatizados

O backend possui uma suíte completa de testes de integração com Jest e Supertest utilizando um banco MongoDB em memória (`mongodb-memory-server`), o que permite rodar todos os testes sem necessidade de ter o MongoDB previamente instalado na máquina:

```bash
cd backend
npm test
```

---

### 4. Execução do Frontend

O frontend é composto por arquivos estáticos e pode ser aberto de qualquer uma das formas abaixo:

- **Opção A (Recomendada — Servidor HTTP simples)**:
  ```bash
  # A partir da pasta raiz ou da pasta frontend:
  npx serve frontend -p 3000
  # ou usando Python:
  # python -m http.server 3000 --directory frontend
  ```
  Acesse no navegador: **`http://localhost:3000`**

- **Opção B (Extensão Live Server do VS Code)**:
  Abra a pasta `frontend` no VS Code, clique com o botão direito no `index.html` e selecione **"Open with Live Server"**.

- **Opção C (Direto pelo navegador)**:
  Abra o arquivo `frontend/index.html` diretamente em seu navegador favorito (Google Chrome, Firefox, Edge).

---

## 🎴 Personagens Iniciais Demonstrativos

| Personagem | Jogo | Lançamento Original | Raridade Demonstrativa | Preço | Placeholder SVG |
|---|---|---|---|---|---|
| **Connor** | Detroit: Become Human | 2018 | Lendária | R$ 89,90 | `connor-detroit.svg` |
| **Arthur Morgan** | Red Dead Redemption 2 | 2018 | Mítica | R$ 119,90 | `arthur-morgan-rdr2.svg` |
| **Doom Slayer** | DOOM (2016) | 2016 | Épica | R$ 69,90 | `doom-slayer-doom.svg` |
| **Bayonetta** | Bayonetta | 2009 | Rara | R$ 49,90 | `bayonetta.svg` |
| **Leon S. Kennedy** | Resident Evil 2 | 1998 | Incomum | R$ 34,90 | `leon-resident-evil.svg` |

---

## 🌐 Configuração de URL da API no Frontend

Caso o frontend e a API sejam hospedados em domínios ou portas diferentes (por exemplo, API na Vercel e frontend no GitHub Pages):

1. Acesse a página **Gerenciar Coleção** (`characters.html`).
2. Clique no botão **`⚙️ API`** no cabeçalho.
3. Insira o endereço do backend (ex: `https://seu-backend.vercel.app/api`).
4. Clique em **Salvar URL**. A alteração é armazenada no `localStorage` e refletida instantaneamente sem modificar arquivos.

---

## ☁️ Guia de Deploy na Vercel & MongoDB Atlas

### 1. Banco de Dados (MongoDB Atlas)
1. Crie uma conta gratuita em [mongodb.com/atlas](https://www.mongodb.com/atlas).
2. Crie um cluster gratuito (M0).
3. Em **Database Access**, crie um usuário com senha.
4. Em **Network Access**, adicione a regra de IP `0.0.0.0/0` (permitir conexões serverless).
5. Copie a string de conexão (ex: `mongodb+srv://usuario:senha@cluster0.mongodb.net/game_cards?retryWrites=true&w=majority`).

### 2. Backend na Vercel
1. Conecte seu repositório no painel da [Vercel](https://vercel.com).
2. Defina o **Root Directory** como `backend`.
3. Adicione as variáveis de ambiente em **Settings > Environment Variables**:
   - `MONGODB_URI`: Sua string de conexão do Atlas.
   - `CORS_ORIGIN`: `*` (ou o domínio do frontend publicado).
   - `NODE_ENV`: `production`
4. Clique em **Deploy**. O arquivo `vercel.json` configurará o roteamento serverless automaticamente.

### 3. Frontend
O frontend pode ser publicado em um projeto separado na Vercel (apontando o Root Directory para `frontend`) ou no GitHub Pages/Netlify. No frontend, basta apontar a URL da API para o endpoint da Vercel.

---

## 📄 Licença

Este projeto é desenvolvido sob a licença **MIT**. Sinta-se livre para utilizar, modificar e aprimorar a aplicação.
