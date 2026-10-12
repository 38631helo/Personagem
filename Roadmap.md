# 🗺️ Roadmap de Desenvolvimento — Game Cards

Este documento apresenta o histórico de marcos de desenvolvimento alcançados no projeto e o planejamento de novas funcionalidades para futuras versões do **Sistema de Cartas de Personagens**.

---

## 📌 Visão Geral do Ciclo de Vida

```text
[Fase 1: Fundação & API] ──► [Fase 2: Galeria & Slider] ──► [Fase 3: Gestão CRUD] ──► [Fase 4: Deploy & Testes] ──► [Fase 5: Expansão]
        (100% Concluído)              (100% Concluído)             (100% Concluído)           (100% Concluído)            (Planejado)
```

---

## ✅ Fases Concluídas

### 🚀 Fase 1: Fundação e API REST (Backend)
- [x] Estruturação modular em `backend/src/`.
- [x] Modelagem do schema `Character` no Mongoose com timestamps automáticos.
- [x] Validação rigorosa dos campos: nome, jogo, data de lançamento, imagem, raridade e preço.
- [x] Implementação dos 6 níveis de raridade (`Comum`, `Incomum`, `Rara`, `Épica`, `Lendária`, `Mítica`).
- [x] Implementação de CRUD completo:
  - `GET /api/characters` (com suporte a busca, filtros e ordenação).
  - `GET /api/characters/:id`.
  - `POST /api/characters`.
  - `PUT /api/characters/:id`.
  - `PATCH /api/characters/:id`.
  - `DELETE /api/characters/:id`.
  - `GET /api/health` (monitoramento de saúde e banco).
- [x] Middleware centralizado de tratamento de erros com respostas JSON semânticas.
- [x] Script de seed manual com proteção contra duplicação de registros iniciais.
- [x] Arquivo `.env.example` sem credenciais sensíveis.

---

### 🎨 Fase 2: Experiência Visual e Slider de Cartas (Frontend)
- [x] Design system moderno com CSS Custom Properties.
- [x] Tema escuro padrão com alternância fluida para tema claro e persistência no `localStorage`.
- [x] Estilização e identidade visual exclusiva para cada nível de raridade (bordas, badges e auras luminosas).
- [x] **Efeito Misterioso de Imagem com Blur**:
  - Imagem desfocada inicialmente (`filter: blur(...)`).
  - Revelação suave ao passar o mouse, tocar na tela ou focar via teclado.
  - Destaque para nome, jogo, `LANÇAMENTO: YYYY`, raridade e preço em reais (`R$ 00,00`).
  - Total acessibilidade para leitores de tela sem aplicar blur ao conteúdo textual.
- [x] Desenvolvimento do componente **Slider Horizontal** em JavaScript puro:
  - Navegação pelas setas anterior/próximo e teclado (`ArrowLeft` / `ArrowRight`).
  - Indicadores interativos de posição (dots).
  - Suporte a toque e deslizamento (touch swipe e mouse drag).
  - Adaptação responsiva para celulares, tablets e desktops.

---

### 📋 Fase 3: Página de Gerenciamento (`characters.html`)
- [x] Interface completa para operações CRUD no frontend.
- [x] Alternância dinâmica entre visualização em **Cards** e **Tabela Responsiva**.
- [x] Busca textual por nome do personagem ou jogo no backend.
- [x] Filtragem dinâmica por raridade.
- [x] Ordenação combinada por preço (menor/maior), lançamento (recente/antigo) e nome.
- [x] Modal de cadastro e edição com validação de campos em tempo real.
- [x] Seleção rápida de placeholders vetoriais locais pelo formulário.
- [x] Modal de confirmação antes de excluir cartas, prevenindo exclusões acidentais.
- [x] Atualização da listagem em tempo real sem recarregamento da página.
- [x] Sistema de notificações toast para feedback visual de sucesso e erro.
- [x] Modal de configuração rápida da URL da API REST (facilita alternar entre localhost e Vercel).

---

### 🛡️ Fase 4: Infraestrutura, Qualidade e Deploy
- [x] Preparação para deploy serverless na **Vercel** (`vercel.json` e connection pool reutilizável em `database.js`).
- [x] Suporte a banco de dados na nuvem com **MongoDB Atlas**.
- [x] Suíte de testes automatizados de integração com Jest e Supertest em `backend/tests/character.test.js`.
- [x] Criação de placeholders locais em SVG vetorial de alta definição para os 5 personagens iniciais e coringa.
- [x] Documentação técnica completa (`api.md`, `Contexto.md`, `Roadmap.md`, `README.md`).

---

## 🔮 Fase 5: Próximas Iterações e Melhorias Futuras

| Funcionalidade | Descrição | Prioridade |
|---|---|---|
| **Deck Builder (Construtor de Baralhos)** | Permitir ao usuário selecionar até 10 cartas para compor um baralho personalizado com cálculo de poder médio e custo total. | Alta |
| **Simulador de Batalha (Mini-Game)** | Mini-game no frontend onde duas cartas duelam com base no ano de lançamento, raridade e atributos calculados. | Média |
| **Efeitos Sonoros Retrô** | Sons sutis no hover e clique de cartas (som de virar carta, brilho para raridade mítica/lendária), com botão para silenciar áudio. | Média |
| **Exportação / Importação (JSON/CSV)** | Permitir exportar a coleção completa em formato JSON ou CSV para backup e importar coleções externas. | Baixa |
| **Autenticação e Múltiplos Colecionadores** | Sistema de login (JWT) permitindo que cada usuário gerencie sua própria coleção privada de cartas. | Baixa |
