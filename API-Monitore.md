# API Monitore — Documentação de Integração

## Visão Geral

O **Monitore** é um serviço de monitoramento contínuo de documentos (CPF/CNPJ). Ao cadastrar um documento, o sistema realiza consultas periódicas e automáticas nas seguintes bases:

| Base | Descrição |
|------|-----------|
| **Receita Federal (RFB)** | Situação cadastral, sócios e capital social |
| **Protestos** | Quantidade de protestos registrados |
| **CCF (Cheques sem Fundo)** | Ocorrências no Cadastro de Emitentes de Cheques sem Fundos |
| **Processos Judiciais** | Quantidade de processos judiciais |
| **Liminares** | Existência de liminares |

Quando há alteração no status monitorado, o sistema dispara alertas automaticamente.

---

## Autenticação

Todas as requisições exigem o parâmetro `apiKey` com sua chave de API.

```
apiKey=SUA_CHAVE_DE_API
```

---

## URL Base

```
https://irql.credithub.com.br
```

---

## Formato das Requisições

A API utiliza uma linguagem de consulta via parâmetro `q` na query string. Os demais parâmetros são enviados também via query string ou body (form-urlencoded).

**Método HTTP:** `POST` ou `GET`

---

## 1. Criar um Monitore

Cadastra um documento (CPF ou CNPJ) para monitoramento contínuo.

### Requisição

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|:-----------:|-----------|
| `apiKey` | string | ✅ | Chave de autenticação |
| `q` | string | ✅ | `INSERT INTO 'FollowDocument'.'Document'` |
| `documento` | string | ✅ | CPF (11 dígitos) ou CNPJ (14 dígitos) |
| `marker` | string | ❌ | Tags separadas por vírgula para organizar seus monitores (ex: `cliente,vip`) |

### Exemplo cURL

```bash
curl -X POST "https://irql.credithub.com.br" \
  -d "apiKey=SUA_CHAVE_DE_API" \
  -d "q=INSERT INTO 'FollowDocument'.'Document'" \
  -d "documento=12345678000199"
```

### Exemplo com marcadores

```bash
curl -X POST "https://irql.credithub.com.br" \
  -d "apiKey=SUA_CHAVE_DE_API" \
  -d "q=INSERT INTO 'FollowDocument'.'Document'" \
  -d "documento=12345678000199" \
  -d "marker=cliente,fornecedor"
```

### Resposta de Sucesso

```json
{
  "document": "12345678000199",
  "state": {},
  "push": {
    "DatabaseFollowDocumentRFB": "...",
    "DatabaseFollowDocumentProtesto": "...",
    "DatabaseFollowDocumentCCF": "...",
    "DatabaseFollowDocumentPDPJ": "...",
    "DatabaseFollowDocumentLiminar": "..."
  },
  "config": {},
  "reference": "...",
  "markers": ["cliente", "fornecedor"],
  "data": {}
}
```

> **Nota:** Se o documento já estiver sendo monitorado, a API retornará os dados do monitore existente sem criar uma duplicata.

---

## 2. Consultar Monitore por Documento

Consulta o status e a situação atual de um documento monitorado específico.

### Requisição

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|:-----------:|-----------|
| `apiKey` | string | ✅ | Chave de autenticação |
| `q` | string | ✅ | `SELECT FROM 'FollowDocument'.'Retrieve'` |
| `documento` | string | ✅ | CPF ou CNPJ monitorado |

### Exemplo cURL

```bash
curl -X POST "https://irql.credithub.com.br" \
  -d "apiKey=SUA_CHAVE_DE_API" \
  -d "q=SELECT FROM 'FollowDocument'.'Retrieve'" \
  -d "documento=12345678000199"
```

### Resposta de Sucesso

```json
{
  "document": "12345678000199",
  "state": {
    "socios": 3,
    "capitalSocial": 100000,
    "protestos": 0,
    "ccf": 0,
    "pdpj": 2
  },
  "push": { ... },
  "config": {},
  "reference": "...",
  "markers": ["cliente", "fornecedor"],
  "data": {}
}
```

### Campos do objeto `state`

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `socios` | int | Quantidade de sócios (apenas CNPJ) |
| `capitalSocial` | int | Capital social em centavos (apenas CNPJ) |
| `protestos` | int | Quantidade total de protestos |
| `ccf` | int | Quantidade de ocorrências de cheques sem fundo |
| `pdpj` | int | Quantidade de processos judiciais |

### Marcadores automáticos (`markers`)

O sistema adiciona marcadores automaticamente conforme as consultas:

| Marcador | Significado |
|----------|-------------|
| `rfb-invalid` | Situação cadastral na Receita Federal **diferente** de ATIVA/REGULAR |
| `has-protesto` | Possui protestos registrados |
| `has-ccf` | Possui ocorrências no CCF |
| `has-pdpj` | Possui processos judiciais |
| `liminar` | Possui liminares |

---

## 3. Listar Todos os Monitores

Retorna a lista completa de documentos monitorados pela sua conta.

### Requisição

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|:-----------:|-----------|
| `apiKey` | string | ✅ | Chave de autenticação |
| `q` | string | ✅ | `SELECT FROM 'FollowDocument'.'List'` |
| `marker` | string | ❌ | Filtra por marcadores (tags separadas por vírgula) |

### Exemplo cURL — Listar todos

```bash
curl -X POST "https://irql.credithub.com.br" \
  -d "apiKey=SUA_CHAVE_DE_API" \
  -d "q=SELECT FROM 'FollowDocument'.'List'"
```

### Exemplo cURL — Filtrar por marcador

```bash
curl -X POST "https://irql.credithub.com.br" \
  -d "apiKey=SUA_CHAVE_DE_API" \
  -d "q=SELECT FROM 'FollowDocument'.'List'" \
  -d "marker=cliente"
```

### Resposta de Sucesso

```json
[
  {
    "document": "12345678000199",
    "state": {
      "socios": 3,
      "capitalSocial": 100000,
      "protestos": 0,
      "ccf": 0,
      "pdpj": 2
    },
    "config": {
      "name": "EMPRESA EXEMPLO LTDA"
    },
    "markers": ["cliente"],
    "push": { ... },
    "reference": "...",
    "data": {}
  },
  {
    "document": "98765432100",
    "state": {
      "protestos": 1,
      "ccf": 0,
      "pdpj": 0
    },
    "config": {
      "name": "JOÃO DA SILVA"
    },
    "markers": ["fornecedor", "has-protesto"],
    "push": { ... },
    "reference": "...",
    "data": {}
  }
]
```

> **Nota:** Na listagem, o campo `config.name` retorna o nome/razão social associado ao documento.

---

## 4. Excluir um Monitore

Remove o monitoramento de um documento.

### Requisição

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|:-----------:|-----------|
| `apiKey` | string | ✅ | Chave de autenticação |
| `q` | string | ✅ | `DELETE FROM 'FollowDocument'.'Document'` |
| `documento` | string | ✅ | CPF ou CNPJ a ser removido |

### Exemplo cURL

```bash
curl -X POST "https://irql.credithub.com.br" \
  -d "apiKey=SUA_CHAVE_DE_API" \
  -d "q=DELETE FROM 'FollowDocument'.'Document'" \
  -d "documento=12345678000199"
```

### Resposta de Sucesso

```json
true
```

### Resposta de Falha (documento não encontrado)

```json
false
```

---

## Códigos de Erro

| Código | Descrição |
|--------|-----------|
| `AUTHENTICATION_FAILURE` | Chave de API inválida ou contrato não suportado |
| `MISSING_ARGUMENT` | Parâmetro obrigatório não enviado (ex: `documento`) |
| `INVALID_ARGUMENT` | Documento com formato inválido (deve ter 11 ou 14 dígitos) |
| `NOT_FOUND` | Documento não encontrado no monitoramento |

### Exemplo de Resposta de Erro

```xml
<?xml version="1.0" encoding="UTF-8"?>
<BPQL>
  <header>
    <exception>Parâmetro necessário - documento</exception>
    <code>MISSING_ARGUMENT</code>
  </header>
</BPQL>
```

---

## Formato do Documento

O parâmetro `documento` aceita CPF e CNPJ com ou sem formatação:

| Formato | Exemplo | Válido |
|---------|---------|:------:|
| CPF sem máscara | `12345678901` | ✅ |
| CPF com máscara | `123.456.789-01` | ✅ |
| CNPJ sem máscara | `12345678000199` | ✅ |
| CNPJ com máscara | `12.345.678/0001-99` | ✅ |

> Internamente, o sistema remove qualquer caractere não numérico.

---

## Resumo Rápido

| Operação | Query (`q`) | Parâmetros |
|----------|-------------|------------|
| **Criar** | `INSERT INTO 'FollowDocument'.'Document'` | `documento`, `marker` (opcional) |
| **Consultar** | `SELECT FROM 'FollowDocument'.'Retrieve'` | `documento` |
| **Listar** | `SELECT FROM 'FollowDocument'.'List'` | `marker` (opcional) |
| **Excluir** | `DELETE FROM 'FollowDocument'.'Document'` | `documento` |

---

## Exemplo Completo de Integração

### 1. Cadastrar documento para monitoramento

```bash
curl -X POST "https://irql.credithub.com.br" \
  -d "apiKey=SUA_CHAVE_DE_API" \
  -d "q=INSERT INTO 'FollowDocument'.'Document'" \
  -d "documento=12345678000199" \
  -d "marker=cliente"
```

### 2. Consultar status do documento

```bash
curl -X POST "https://irql.credithub.com.br" \
  -d "apiKey=SUA_CHAVE_DE_API" \
  -d "q=SELECT FROM 'FollowDocument'.'Retrieve'" \
  -d "documento=12345678000199"
```

### 3. Listar todos os monitores

```bash
curl -X POST "https://irql.credithub.com.br" \
  -d "apiKey=SUA_CHAVE_DE_API" \
  -d "q=SELECT FROM 'FollowDocument'.'List'"
```

### 4. Remover documento do monitoramento

```bash
curl -X POST "https://irql.credithub.com.br" \
  -d "apiKey=SUA_CHAVE_DE_API" \
  -d "q=DELETE FROM 'FollowDocument'.'Document'" \
  -d "documento=12345678000199"
```

---

**Dúvidas?** Entre em contato com o suporte CreditHub.
