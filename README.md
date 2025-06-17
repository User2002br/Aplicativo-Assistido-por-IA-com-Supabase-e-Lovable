# Aplicativo-Assistido-por-IA-com-Supabase-e-Lovable

## 📘 1. Documentação
markdown
Copiar
Editar
# API REST – Tabela `tarefas` (Supabase)

Base URL: `postgresql://postgres:[YOUR-PASSWORD]@db.exjejvadsweaqqxiejsx.supabase.co:5432/postgres`

## 🔐 Autenticação

Todos os endpoints exigem:

- Header `apikey`: sua chave anônima pública
- Header `Authorization`: `Bearer <token_jwt>` (se RLS estiver ativado)

---

## 📥 Criar uma tarefa

**POST** `/rest/v1/tarefas`

## Requisição:
```json
{
  "usuario_id": 101,
  "titulo": "Finalizar relatório",
  "descricao": "Relatório mensal de vendas",
  "prioridade": "Alta",
  "status": "Em andamento",
  "data_limite": "2025-06-20"
}

```
Resposta:
201 Created com a tarefa criada

## 📤 Listar tarefas
GET /rest/v1/tarefas

## Exemplo:
http
Copiar
Editar
GET /rest/v1/tarefas?usuario_id=eq.101&order=created_at.desc
Resposta:
json
Copiar
Editar

```
[
  {
    "id": 1,
    "usuario_id": 101,
    "titulo": "Finalizar relatório",
    "descricao": "Relatório mensal de vendas",
    "prioridade": "Alta",
    "status": "Em andamento",
    "data_limite": "2025-06-20",
    "created_at": "2025-06-10T00:00:00Z"
  }
]

```
## ✏️ Atualizar tarefa
PATCH /rest/v1/tarefas?id=eq.1

Requisição:
json
Copiar
Editar
```
{
  "status": "Concluído"
}
```
Resposta:
204 No Content (sem conteúdo, sucesso)

## 🗑️ Deletar tarefa
```
DELETE /rest/v1/tarefas?id=eq.1
```
Remove a tarefa com id = 1

Resposta: 204 No Content

📌 Campos disponíveis
Campo	Tipo	Descrição
id	integer	Identificador da tarefa
usuario_id	integer	ID do usuário responsável
titulo	text	Título da tarefa
descricao	text	Descrição detalhada
prioridade	text	Nível de prioridade
status	text	Status atual (ex: Pendente)
data_limite	date	Data de vencimento
created_at	timestamp	Data de criação automática

bash
Copiar
Editar

---

## 📄 2. OpenAPI (Swagger) – JSON

```json
{
  "openapi": "3.0.0",
  "info": {
    "title": "Supabase API - Tabela tarefas",
    "version": "1.0.0"
  },
  "paths": {
    "/rest/v1/tarefas": {
      "get": {
        "summary": "Listar tarefas",
        "parameters": [
          {
            "name": "usuario_id",
            "in": "query",
            "schema": {
              "type": "integer"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Lista de tarefas"
          }
        }
      },
      "post": {
        "summary": "Criar nova tarefa",
        "requestBody": {
          "required": true,
          "content": {
            "application/json": {
              "schema": {
                "type": "object",
                "properties": {
                  "usuario_id": { "type": "integer" },
                  "titulo": { "type": "string" },
                  "descricao": { "type": "string" },
                  "prioridade": { "type": "string" },
                  "status": { "type": "string" },
                  "data_limite": { "type": "string", "format": "date" }
                },
                "required": ["usuario_id", "titulo"]
              }
            }
          }
        },
        "responses": {
          "201": { "description": "Criado com sucesso" }
        }
      }
    },
    "/rest/v1/tarefas?id=eq.{id}": {
      "patch": {
        "summary": "Atualizar tarefa",
        "parameters": [
          {
            "name": "id",
            "in": "path",
            "required": true,
            "schema": { "type": "integer" }
          }
        ],
        "requestBody": {
          "required": true,
          "content": {
            "application/json": {
              "schema": {
                "type": "object",
                "properties": {
                  "status": { "type": "string" }
                }
              }
            }
          }
        },
        "responses": {
          "204": { "description": "Atualizado com sucesso" }
        }
      },
      "delete": {
        "summary": "Excluir tarefa",
        "parameters": [
          {
            "name": "id",
            "in": "path",
            "required": true,
            "schema": { "type": "integer" }
          }
        ],
        "responses": {
          "204": { "description": "Deletado com sucesso" }
        }
      }
    }
  }
}
