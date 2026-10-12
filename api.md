# 📡 Documentação da API REST — Game Cards

Esta API REST fornece os serviços para listar, consultar, cadastrar, atualizar e excluir cartas colecionáveis de personagens de videogame com persistência no MongoDB.

A API foi projetada para execução em servidores locais Node.js ou em ambientes serverless (Vercel).

---

## 🌐 URL Base

- **Ambiente Local:** `http://localhost:5000/api`
- **Ambiente de Produção (Vercel):** `https://seu-backend.vercel.app/api`

---

## 📋 Resumo dos Endpoints

| Método | Endpoint | Descrição |
|---|---|---|
| `GET` | `/api/health` | Verifica a saúde operacional da API e a conexão com o banco |
| `GET` | `/api/characters` | Lista personagens com filtros, busca textual e ordenação |
| `GET` | `/api/characters/:id` | Retorna os detalhes de um personagem específico por ID |
| `POST` | `/api/characters` | Cadastra um novo personagem |
| `PUT` | `/api/characters/:id` | Atualiza todos os campos de um personagem existente |
| `PATCH` | `/api/characters/:id` | Atualiza parcialmente os campos informados de um personagem |
| `DELETE` | `/api/characters/:id` | Remove um personagem pelo ID |

---

## 🩺 1. Health Check

### `GET /api/health`
Retorna o status operacional da aplicação e o estado da conexão com o MongoDB.

#### Resposta de Sucesso (`200 OK`)
```json
{
  "status": "healthy",
  "message": "API do Sistema de Cartas de Personagens está operacional.",
  "database": "connected",
  "timestamp": "2026-10-12T13:00:00.000Z",
  "uptime": 45.2,
  "environment": "development"
}
```

---

## 🃏 2. Listagem de Personagens

### `GET /api/characters`
Retorna a lista de cartas de personagens. Suporta parâmetros de consulta opcionais para busca, filtragem e ordenação.

#### Query Parameters

| Parâmetro | Tipo | Descrição | Exemplo |
|---|---|---|---|
| `search` | String | Busca textual em `name` e `game` (case-insensitive) | `?search=Detroit` |
| `name` | String | Filtro específico por nome | `?name=Connor` |
| `game` | String | Filtro específico por jogo | `?game=DOOM` |
| `rarity` | String | Filtro por raridade (`Comum`, `Incomum`, `Rara`, `Épica`, `Lendária`, `Mítica`) | `?rarity=Lendária` |
| `sortBy` | String | Campo para ordenação (`createdAt`, `price`, `releaseDate`, `name`, `game`) | `?sortBy=price` |
| `order` | String | Direção da ordenação (`asc` ou `desc`). Padrão: `desc` | `?order=asc` |
| `page` | Número | Número da página (ativa paginação) | `?page=1` |
| `limit` | Número | Quantidade de itens por página | `?limit=10` |

#### Resposta de Sucesso (`200 OK`)
```json
{
  "success": true,
  "count": 5,
  "total": 5,
  "data": [
    {
      "id": "670aa1b820a109419b48c701",
      "name": "Connor",
      "game": "Detroit: Become Human",
      "releaseDate": "2018-05-25T00:00:00.000Z",
      "imageUrl": "assets/placeholders/connor-detroit.svg",
      "rarity": "Lendária",
      "price": 89.9,
      "description": "Androide modelo RK800 projetado pela CyberLife...",
      "createdAt": "2026-10-12T13:00:00.000Z",
      "updatedAt": "2026-10-12T13:00:00.000Z"
    }
  ]
}
```

---

## 🔍 3. Detalhes de um Personagem

### `GET /api/characters/:id`
Busca e retorna um único personagem pelo seu identificador do MongoDB.

#### Parâmetros de Rota
- `id`: ObjectId de 24 caracteres hexadecimais.

#### Resposta de Sucesso (`200 OK`)
```json
{
  "success": true,
  "data": {
    "id": "670aa1b820a109419b48c701",
    "name": "Connor",
    "game": "Detroit: Become Human",
    "releaseDate": "2018-05-25T00:00:00.000Z",
    "imageUrl": "assets/placeholders/connor-detroit.svg",
    "rarity": "Lendária",
    "price": 89.9,
    "description": "Androide modelo RK800 projetado pela CyberLife...",
    "createdAt": "2026-10-12T13:00:00.000Z",
    "updatedAt": "2026-10-12T13:00:00.000Z"
  }
}
```

#### Respostas de Erro
- **`400 Bad Request`**: ID com formato inválido.
  ```json
  {
    "success": false,
    "error": "O ID fornecido 'id-invalido' não é um identificador válido do MongoDB."
  }
  ```
- **`404 Not Found`**: Personagem não encontrado.
  ```json
  {
    "success": false,
    "error": "Personagem não encontrado."
  }
  ```

---

## ➕ 4. Cadastro de Personagem

### `POST /api/characters`
Cadastra uma nova carta no banco de dados.

#### Cabeçalhos Obrigatórios
- `Content-Type: application/json`

#### Corpo da Requisição (`application/json`)
```json
{
  "name": "Arthur Morgan",
  "game": "Red Dead Redemption 2",
  "releaseDate": "2018-10-26",
  "imageUrl": "assets/placeholders/arthur-morgan-rdr2.svg",
  "rarity": "Mítica",
  "price": 119.90,
  "description": "Fora da lei experiente e braço direito da gangue Van der Linde."
}
```

#### Regras de Validação:
- `name`: String obrigatória (2 a 100 caracteres).
- `game`: String obrigatória (2 a 120 caracteres).
- `releaseDate`: Data válida no formato `AAAA-MM-DD` ou `AAAA`. Armazenada em UTC para evitar discrepâncias de fuso.
- `imageUrl`: URL absoluta (`http://...`, `https://...`) ou caminho relativo (`assets/placeholders/...`).
- `rarity`: Deve ser uma das 6 opções: `Comum`, `Incomum`, `Rara`, `Épica`, `Lendária`, `Mítica`.
- `price`: Número maior ou igual a zero (aceita decimais).
- `description`: String opcional (máx. 1000 caracteres).

#### Resposta de Sucesso (`201 Created`)
```json
{
  "success": true,
  "message": "Personagem cadastrado com sucesso!",
  "data": {
    "id": "670aa1b820a109419b48c702",
    "name": "Arthur Morgan",
    "game": "Red Dead Redemption 2",
    "releaseDate": "2018-10-26T00:00:00.000Z",
    "imageUrl": "assets/placeholders/arthur-morgan-rdr2.svg",
    "rarity": "Mítica",
    "price": 119.9,
    "description": "Fora da lei experiente e braço direito da gangue Van der Linde.",
    "createdAt": "2026-10-12T13:05:00.000Z",
    "updatedAt": "2026-10-12T13:05:00.000Z"
  }
}
```

---

## ✏️ 5. Atualização Completa

### `PUT /api/characters/:id`
Substitui todos os atributos editáveis da carta existente.

#### Corpo da Requisição (`application/json`)
```json
{
  "name": "Arthur Morgan — Edição Especial",
  "game": "Red Dead Redemption 2",
  "releaseDate": "2018-10-26",
  "imageUrl": "assets/placeholders/arthur-morgan-rdr2.svg",
  "rarity": "Mítica",
  "price": 129.90,
  "description": "Edição comemorativa Van der Linde."
}
```

#### Resposta de Sucesso (`200 OK`)
```json
{
  "success": true,
  "message": "Personagem atualizado com sucesso!",
  "data": {
    "id": "670aa1b820a109419b48c702",
    "name": "Arthur Morgan — Edição Especial",
    "game": "Red Dead Redemption 2",
    "releaseDate": "2018-10-26T00:00:00.000Z",
    "imageUrl": "assets/placeholders/arthur-morgan-rdr2.svg",
    "rarity": "Mítica",
    "price": 129.9,
    "description": "Edição comemorativa Van der Linde.",
    "createdAt": "2026-10-12T13:05:00.000Z",
    "updatedAt": "2026-10-12T13:10:00.000Z"
  }
}
```

---

## 🔄 6. Atualização Parcial

### `PATCH /api/characters/:id`
Altera apenas os atributos específicos passados no corpo da requisição.

#### Corpo da Requisição (`application/json`)
```json
{
  "price": 85.50
}
```

#### Resposta de Sucesso (`200 OK`)
```json
{
  "success": true,
  "message": "Personagem atualizado com sucesso!",
  "data": {
    "id": "670aa1b820a109419b48c702",
    "name": "Arthur Morgan",
    "price": 85.5,
    "updatedAt": "2026-10-12T13:12:00.000Z"
  }
}
```

---

## 🗑️ 7. Exclusão de Personagem

### `DELETE /api/characters/:id`
Exclui permanentemente a carta do banco de dados.

#### Resposta de Sucesso (`200 OK`)
```json
{
  "success": true,
  "message": "Personagem 'Arthur Morgan' excluído com sucesso!",
  "data": {
    "id": "670aa1b820a109419b48c702",
    "name": "Arthur Morgan"
  }
}
```

---

## ⚠️ Códigos HTTP e Padrão de Erro

A API utiliza códigos HTTP semânticos e respostas estruturadas:

| Código | Significado | Exemplo de Ocorrência |
|---|---|---|
| `200` | OK | Requisições GET, PUT, PATCH e DELETE bem-sucedidas |
| `201` | Created | Cadastro com POST bem-sucedido |
| `400` | Bad Request | Dados inválidos, campos obrigatórios ausentes ou ID com formato incorreto |
| `404` | Not Found | Recurso não localizado para o ID informado |
| `409` | Conflict | Registro duplicado em campo único |
| `500` | Internal Server Error | Erro inesperado no servidor |
| `503` | Service Unavailable | Banco de dados MongoDB indisponível |

### Estrutura Padrão de Erro
```json
{
  "success": false,
  "error": "Erro de validação dos dados enviados.",
  "details": [
    {
      "field": "price",
      "message": "O preço deve ser igual ou superior a zero."
    }
  ]
}
```
