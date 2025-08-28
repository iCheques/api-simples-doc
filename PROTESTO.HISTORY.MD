# Protestos API – Histórico

Documentação completa, autoexplicativa e segura com exemplos **mockados**.

> Esta documentação usa dados fictícios. Não expõe informações reais de clientes.
> O endpoint consulta o histórico de protestos consolidado no Data Lake para um CPF ou CNPJ por meio de uma query IRQL.

---

## Visão geral

A API retorna um mapa de registros, onde cada propriedade do objeto é a **chave única** do protesto e o valor é um **objeto com os atributos do protesto**.
A consulta é feita com um `POST` para o endpoint raíz usando `application/x-www-form-urlencoded`.

- Base URL: `https://irql.credithub.com.br`
- Método: `POST /`
- Autenticação: `apiKey` via querystring ou corpo `x-www-form-urlencoded`
- Query IRQL suportada para histórico:
  `SELECT FROM 'Protestos'.'History'`

---

## Segurança

- `apiKey` no corpo da requisição ou na querystring.
- Enviar somente por HTTPS.
- Trate a apiKey como segredo. Não versionar em repositórios públicos.

---

## Endpoint e parâmetros

### POST `/`

**Headers**

```
Content-Type: application/x-www-form-urlencoded
```

**Body (x-www-form-urlencoded)**

| Campo       | Tipo   | Obrigatório | Exemplo                             | Descrição                                        |
| ----------- | ------ | ----------- | ----------------------------------- | ------------------------------------------------ |
| `apiKey`    | string | Sim         | `sk_live_abc123...`                 | Chave de API válida.                             |
| `q`         | string | Sim         | `SELECT FROM 'Protestos'.'History'` | Comando IRQL suportado para histórico.           |
| `documento` | string | Sim         | `00000000000000`                    | CPF ou CNPJ, com ou sem máscara. Somente Brasil. |

---

## Exemplos de uso

### cURL

```bash
curl -X POST "https://irql.credithub.com.br/" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  --data-urlencode "apiKey=<YOUR_API_KEY>" \
  --data-urlencode "q=SELECT FROM 'Protestos'.'History'" \
  --data-urlencode "documento=00000000000100"
```

### JavaScript com `fetch`

```js
const params = new URLSearchParams({
  apiKey: process.env.CREDITHUB_API_KEY,
  q: "SELECT FROM 'Protestos'.'History'",
  documento: "00000000000100",
});

const res = await fetch("https://irql.credithub.com.br/", {
  method: "POST",
  headers: { "Content-Type": "application/x-www-form-urlencoded" },
  body: params,
});

if (!res.ok) throw new Error(`HTTP ${res.status}`);
const data = await res.json();
console.log(data);
```

### Node.js com Axios

```js
import axios from "axios";
import qs from "qs";

const body = qs.stringify({
  apiKey: process.env.CREDITHUB_API_KEY,
  q: "SELECT FROM 'Protestos'.'History'",
  documento: "00000000000100",
});

const { data } = await axios.post("https://irql.credithub.com.br/", body, {
  headers: { "Content-Type": "application/x-www-form-urlencoded" },
});

console.log(data);
```

### Python com `requests`

```python
import os
import requests

payload = {
  "apiKey": os.environ["CREDITHUB_API_KEY"],
  "q": "SELECT FROM 'Protestos'.'History'",
  "documento": "00000000000000"
}

r = requests.post("https://irql.credithub.com.br/", data=payload, timeout=30)
r.raise_for_status()
data = r.json()
print(data)
```

### PHP com `curl`

```php
<?php
$payload = http_build_query([
  'apiKey' => getenv('CREDITHUB_API_KEY'),
  'q' => "SELECT FROM 'Protestos'.'History'",
  'documento' => '00000000000100'
]);

$ch = curl_init("https://irql.credithub.com.br/");
curl_setopt_array($ch, [
  CURLOPT_POST => true,
  CURLOPT_POSTFIELDS => $payload,
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_HTTPHEADER => ['Content-Type: application/x-www-form-urlencoded'],
  CURLOPT_TIMEOUT => 30
]);

$response = curl_exec($ch);
if ($response === false) {
  throw new RuntimeException(curl_error($ch));
}
$httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
curl_close($ch);

if ($httpCode !== 200) {
  throw new RuntimeException("HTTP $httpCode: $response");
}

$data = json_decode($response, true);
print_r($data);
```

---

## Resposta esperada

A resposta é um objeto JSON onde **cada chave** é o identificador único do protesto.
Abaixo um exemplo totalmente **mockado**.

```json
{
  "99999999999999999999999": {
    "cpfCnpj": "00000000000000",
    "data": "15/05/2024",
    "dataProtesto": "15/05/2024",
    "valor": 1234.56,
    "valorProtestado": 1234.56,
    "temAnuencia": false,
    "nomeApresentante": "PROCURADORIA GERAL",
    "nomeCedente": "FAZENDA",
    "chave": "99999999999999999999999",
    "creation": "28/08/2025",
    "custas": 100.0,
    "missingAt": null,
    "protestoProvavelmenteExpirado": false
  },
  "88888888888888888888888": {
    "cpfCnpj": "00000000000000",
    "data": "16/05/2024",
    "dataProtesto": "16/05/2024",
    "valor": 9876.54,
    "valorProtestado": 9876.54,
    "temAnuencia": true,
    "nomeApresentante": "PROCURADORIA GERAL",
    "nomeCedente": "FAZENDA",
    "chave": "88888888888888888888888",
    "creation": "28/08/2025",
    "custas": 250.4,
    "missingAt": null,
    "protestoProvavelmenteExpirado": false
  }
}
```

---

## Esquema dos campos

| Campo                           | Tipo    | Descrição                                                       |
| ------------------------------- | ------- | --------------------------------------------------------------- |
| `cpfCnpj`                       | string  | Documento do devedor. Apenas dígitos.                           |
| `data`                          | string  | Data do registro no formato dd/MM/yyyy.                         |
| `dataProtesto`                  | string  | Data do protesto no formato dd/MM/yyyy.                         |
| `valor`                         | number  | Valor da dívida registrada.                                     |
| `valorProtestado`               | number  | Valor protestado oficialmente.                                  |
| `temAnuencia`                   | boolean | Indica se houve anuência do credor para baixa.                  |
| `nomeApresentante`              | string  | Nome do apresentante do protesto.                               |
| `nomeCedente`                   | string  | Nome do cedente.                                                |
| `chave`                         | string  | Identificador único do protesto.                                |
| `creation`                      | string  | Data de criação do registro no sistema, dd/MM/yyyy.             |
| `custas`                        | number  | Valor de custas processuais.                                    |
| `missingAt`                     | string  | Data em que o registro foi marcado como ausente. Pode ser nulo. |
| `protestoProvavelmenteExpirado` | boolean | Indicador heurístico se o protesto provavelmente expirou.       |

---

## Boas práticas de integração

1. **Normalização de documento**
   Aceitamos com ou sem máscara. Para consistência, normalize internamente removendo não dígitos antes de salvar em seu banco.

2. **Tratamento de moeda**
   Os campos `valor`, `valorProtestado` e `custas` são numéricos em notação ISO. Para exibir, formate conforme locale.

3. **Idempotência de leitura**
   A chamada é de leitura. Se precisar versionar resultados historicamente, armazene também um timestamp da consulta.

4. **Resiliência**
   Implemente `timeout` de 30 s e uma política de retry leve para erros transitórios 5xx. Evite retries para 4xx.

5. **Observabilidade**
   Logue `documento` normalizado, `status HTTP`, latência e hash da resposta para deduplicação.

---

## Erros comuns

| HTTP | Causa                                | Ação sugerida                                  |
| ---- | ------------------------------------ | ---------------------------------------------- |
| 400  | Parâmetros ausentes ou malformados   | Verifique `apiKey`, `q` e `documento`.         |
| 401  | `apiKey` ausente ou inválida         | Reenvie com chave válida.                      |
| 403  | Permissão insuficiente               | Contate o administrador para habilitar acesso. |
| 404  | Rota não encontrada                  | Verifique a URL base e método.                 |
| 429  | Limite de taxa atingido              | Aplique backoff exponencial.                   |
| 500  | Erro interno                         | Tente novamente e monitore.                    |
| 503  | Serviço indisponível momentaneamente | Retry com jitter.                              |

### Mensagens de erro de exemplo

```json
{
  "error": {
    "code": "invalid_request",
    "message": "Parâmetro 'documento' é obrigatório."
  }
}
```

```json
{
  "error": {
    "code": "unauthorized",
    "message": "apiKey inválida."
  }
}
```

---

## Especificação OpenAPI 3.0

> Copie e cole em Swagger Editor, Redocly ou Postman. Os exemplos abaixo são fictícios.

```yaml
openapi: 3.0.3
info:
  title: Protestos API – Histórico
  version: "1.0.0"
  description: >
    Consulta o histórico de protestos no Data Lake.
    Exemplos mockados para evitar exposição de dados reais.
servers:
  - url: https://irql.credithub.com.br
tags:
  - name: Protestos
paths:
  /:
    post:
      tags: [Protestos]
      operationId: protestosHistoryQuery
      summary: Consulta histórico de protestos por CPF ou CNPJ
      requestBody:
        required: true
        content:
          application/x-www-form-urlencoded:
            schema:
              type: object
              required: [apiKey, q, documento]
              properties:
                apiKey:
                  type: string
                q:
                  type: string
                  enum: ["SELECT FROM 'Protestos'.'History'"]
                documento:
                  type: string
                  example: "00.000.000/0001-00"
            examples:
              mock:
                value:
                  apiKey: "<YOUR_API_KEY>"
                  q: "SELECT FROM 'Protestos'.'History'"
                  documento: "00.000.000/0001-00"
      responses:
        "200":
          description: Sucesso
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/HistoryResponse"
              examples:
                mock:
                  value:
                    "99999999999999999999999":
                      cpfCnpj: "00000000000000"
                      data: "15/05/2024"
                      dataProtesto: "15/05/2024"
                      valor: 1234.56
                      valorProtestado: 1234.56
                      temAnuencia: false
                      nomeApresentante: "PROCURADORIA GERAL (MOCK)"
                      nomeCedente: "FAZENDA (MOCK)"
                      chave: "99999999999999999999999"
                      creation: "28/08/2025"
                      custas: 100.0
                      missingAt: null
                      protestoProvavelmenteExpirado: false
        "400": { description: Requisição inválida }
        "401": { description: Não autorizado }
        "500": { description: Erro interno }
      security:
        - ApiKeyQuery: []
components:
  securitySchemes:
    ApiKeyQuery:
      type: apiKey
      in: query
      name: apiKey
  schemas:
    HistoryResponse:
      type: object
      additionalProperties:
        $ref: "#/components/schemas/HistoryItem"
    HistoryItem:
      type: object
      properties:
        cpfCnpj: { type: string, example: "00000000000000" }
        data: { type: string, example: "16/05/2024" }
        dataProtesto: { type: string, example: "16/05/2024" }
        valor: { type: number, format: double, example: 1234.56 }
        valorProtestado: { type: number, format: double, example: 1234.56 }
        temAnuencia: { type: boolean, example: false }
        nomeApresentante: { type: string, example: "APRESENTANTE (MOCK)" }
        nomeCedente: { type: string, example: "CEDENTE (MOCK)" }
        chave: { type: string, example: "99999999999999999999999" }
        creation: { type: string, example: "28/08/2025" }
        custas: { type: number, format: double, example: 100.0 }
        missingAt: { type: string, nullable: true, example: null }
        protestoProvavelmenteExpirado: { type: boolean, example: false }
```

---

## Mapeamento de status HTTP no cliente

```ts
type ProtestosApiError =
  | { type: "BadRequest"; status: 400; message: string }
  | { type: "Unauthorized"; status: 401; message: string }
  | { type: "Forbidden"; status: 403; message: string }
  | { type: "TooManyRequests"; status: 429; message: string }
  | { type: "ServerError"; status: 500 | 503; message: string };
```

---

## Testes e mocking local

- Use `nock` no Node.js ou `responses` no Python para simular o endpoint.
- Exemplo Node com `nock`:

```js
import nock from "nock";

nock("https://irql.credithub.com.br")
  .post("/")
  .reply(200, {
    "99999999999999999999999": {
      cpfCnpj: "00000000000000",
      data: "15/05/2024",
      dataProtesto: "15/05/2024",
      valor: 1234.56,
      valorProtestado: 1234.56,
      temAnuencia: false,
      nomeApresentante: "PROCURADORIA GERAL (MOCK)",
      nomeCedente: "FAZENDA (MOCK)",
      chave: "99999999999999999999999",
      creation: "28/08/2025",
      custas: 100.0,
      missingAt: null,
      protestoProvavelmenteExpirado: false,
    },
  });
```

---

## Dicas de troubleshooting

- Cheque se o `q` enviado é exatamente `SELECT FROM 'Protestos'.'History'`.
- Valide o documento. Tente enviar somente dígitos se obtiver 400.
- Verifique clock skew. Se o servidor de origem estiver com horário incorreto pode resultar em falhas intermitentes.
- Monitore latência e trate timeouts com retry controlado.

---

## Versionamento e mudanças

- `v1.0.0`
  Primeira versão pública da documentação do Histórico de Protestos.

--
