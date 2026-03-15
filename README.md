## Documentação da API de Consulta Simples JSON PJ/PF CreditHub 🚀

A API de Consulta de CNPJ/CPF CreditHub é a ferramenta ideal para obter informações detalhadas e atualizadas sobre empresas (CNPJ) e pessoas físicas (CPF) no Brasil. 💼 Com ela, você pode integrar dados relevantes aos seus sistemas e tomar decisões mais assertivas. 🎯

## Índice

1.  **Introdução**
    - O que é a API CreditHub?
    - Para que serve?
    - Quem pode usar?
    - Benefícios
2.  **Como utilizar**
    - Autenticação
    - Endpoint
    - Método
    - Parâmetros
    - Parâmetros Adicionais
    - Cabeçalhos (Headers)
    - Exemplos de Requisição
    - Formato de Resposta
    - Campos da Resposta
      - Campos Específicos para CNPJ
      - Campos Específicos para CPF
      - Campos Comuns para CNPJ e CPF
    - PEFIN/REFIN Serasa (PF/PJ)
      - Estrutura Geral
      - Descrição dos Campos
        - Dados Cadastrais (`user`)
        - Pendências Financeiras (`bello`)
        - Resumo das Pendências
    - PEFIN/REFIN Boa Vista (PF/PJ)
      - Estrutura Geral
      - Campos Principais
      - Detalhes dos Campos em `dadosCadastrais`
      - Detalhes dos Campos em `spc`
    - Consulta Veículos
      - Estrutura JSON de Veículos
      - Descrição dos Campos
      - Observações

### 1. Introdução

**O que é a API CreditHub?** 🤔

A API CreditHub é uma interface de programação de aplicações (API) que permite consultar informações sobre empresas e pessoas físicas no Brasil. Ela fornece dados como razão social, nome fantasia, endereço, situação cadastral, entre outros.💼

**Para que serve?** 🔍

A API CreditHub é útil para diversas finalidades, como:

**-> Análise de crédito:** Avalie a saúde financeira de empresas e pessoas físicas antes de conceder crédito.💰
**-> Verificação de dados cadastrais:** Confirme a veracidade das informações fornecidas por clientes e parceiros.✅
**-> Enriquecimento de cadastros:** Adicione informações relevantes aos seus cadastros de clientes e prospects.📊
**-> Prevenção à fraude:** Identifique possíveis fraudes e reduza riscos.🛡️
**-> Compliance:** Garanta o cumprimento de normas e regulamentações.⚖️

**Quem pode usar?** 👥

Qualquer pessoa ou empresa que precise de informações sobre CNPJs ou CPFs pode usar a API CreditHub. Desenvolvedores, analistas de crédito e compliance são alguns exemplos de usuários que podem se beneficiar desta ferramenta. 👨‍💻👩‍💼

**Benefícios:** ✨

**-> Dados atualizados:** A API acessa fontes oficiais e confiáveis, como a Receita Federal, para garantir a precisão e atualidade das informações. 🔄
**-> Facilidade de integração:** A API é RESTful e retorna dados em formato JSON, facilitando a integração com seus sistemas existentes. 🤝
**-> Flexibilidade:** Você pode consultar CNPJs e CPFs individualmente ou em lote, de acordo com suas necessidades. 🔀
**-> Cobertura nacional:** A API abrange todo o território brasileiro, fornecendo informações sobre empresas e pessoas físicas de todos os estados. 🇧🇷
**-> Suporte a consultas assíncronas:** A API oferece suporte a consultas assíncronas, permitindo que você receba notificações quando os resultados da consulta estiverem prontos. 🔔

### 2. Como utilizar 🛠️

Devido à natureza assíncrona da consulta, a cada requisição à URL, novos dados podem estar disponíveis. Para receber atualizações contínuas sobre o status da consulta e obter os dados completos assim que estiverem prontos, utilize o parâmetro opcional `callback`. 🔄 Ao fornecer uma URL válida como valor para callback, você receberá uma notificação nessa URL assim que a consulta for concluída ou houverem novas informações relevantes.📬

### Autenticação 🔐

Para utilizar a API CreditHub, você precisa de uma chave de API. 🔑

**Como obter sua chave de API:**

**1.** Para obter sua chave de API, entre em contato com nossa equipe comercial através do e-mail contato@credithub.com.br. 🤝
**2.** Nossa equipe irá auxiliá-lo no processo de aquisição da chave e fornecer todas as informações necessárias para começar a utilizar a API CreditHub. 📧

**Importante:** Mantenha sua chave de API em segurança, pois ela é a sua credencial de acesso à API CreditHub. 🔒

### Endpoint

```
https://irql.credithub.com.br/simples/SUA_API_KEY/CNPJ_OU_CPF_A_SER_PESQUISADO
```

### Método

```
GET
```

### Parâmetros

- `SUA_API_KEY` (obrigatório): Sua chave de API fornecida pelo CreditHub.
- `CNPJ_OU_CPF_A_SER_PESQUISADO` (obrigatório): O número de CNPJ da empresa ou CPF da pessoa a ser pesquisado.

### Parâmetros Adicionais

- `serasa=ture` (opcional): retorna informações de PEFIN/REFIN Serasa;
- `boavista=true` retorna informações de PEFIN/REFIN Boa Vista.
- `veiculos` (opcional): Se definido como `true`, retorna informações sobre veículos.
- `callback` (opcional): URL que receberá os JSONs com os dados atualizados (notificações de andamento da consulta).

### Cabeçalhos (Headers)

- `Content-Type: application/json` (opcional): Indica que a resposta esperada é em formato JSON.

### Exemplos de Requisição

**cURL:**

```bash
curl --location 'https://irql.credithub.com.br/simples/abcdef12345/08075274000402' \
--header 'Content-Type: application/json'
```

**JavaScript (usando fetch):**

```javascript
fetch("https://irql.credithub.com.br/simples/abcdef12345/08075274000402", {
  method: "GET",
  headers: {
    "Content-Type": "application/json",
  },
})
  .then((response) => {
    if (!response.ok) {
      throw new Error("Erro na requisição");
    }
    return response.json();
  })
  .then((data) => {
    console.log(data); // Dados da consulta em JSON
  })
  .catch((error) => {
    console.error("Erro:", error);
  });
```

### Formato de Resposta

**Em Caso de Sucesso (JSON):**

```json
{
  "data": {
    // ... (campos da consulta, como cnpj, capitalSocial, razaoSocial, etc.)
  }
}
```

**Em Caso de Erro (XML):**

```xml
<BPQL>
    <header>
        <description>Internal Server Error</description>
        <date-time>04/07/2024 12:37:38</date-time>
        <exception id="GENERICO" code="0" source="GENERICO">É necessário uma chave de acesso</exception>
    </header>
    <body/>
</BPQL>
```

### Campos da Resposta

**Para Consultas CNPJ:**

- `cnpj`: Número do CNPJ da empresa.
- `capitalSocial`: Valor do capital social da empresa.
- `razaoSocial`: Razão social da empresa.
- `nomeFantasia`: Nome fantasia da empresa.
- `dataAbertura`: Data de abertura da empresa.
- `idadeEmpresa`: Idade da empresa calculada em anos.
- `faixaIdade`: Faixa etária da empresa.
- `quantidadeFuncionarios`: Número de funcionários da empresa.
- `faixaFuncionarios`: Faixa de quantidade de funcionários da empresa.
- `porteEmpresa`: Porte da empresa (MEI, Microempresa, Empresa de Pequeno Porte, etc.).
- `receitaStatus`: Status na Receita Federal.
- `dataReceitaStatus`: Data da última atualização do status na Receita Federal.
- `cnae`: Código CNAE principal da empresa.
- `cnaeDescricao`: Descrição do código CNAE principal.
- `cnaeGrupo`: Grupo CNAE ao qual a empresa pertence.
- `cnaeSubgrupo`: Subgrupo CNAE ao qual a empresa pertence.
- `tipoEmpresa`: Tipo de empresa (ex.: Sociedade Limitada, Empresa Individual, etc.).
- `naturezaJuridica`: Natureza jurídica da empresa.
- `site`: Website da empresa, se disponível.
- `regimeTributario`: Regime tributário da empresa (Simples Nacional, Lucro Presumido, etc.).
- `enderecos`: Lista de endereços da empresa.
- `telefones`: Lista de números de telefone da empresa.
- `emails`: Lista de endereços de email da empresa.
- `cnaesSecundarios`: Lista de códigos CNAE secundários.
- `quadroSocietario`: Composição do quadro societário da empresa.

**Para Consultas CPF:**

- `cpf`: Número do CPF da pessoa.
- `nome`: Nome completo da pessoa.
- `sexo`: Sexo da pessoa.
- `dataNascimento`: Data de nascimento da pessoa.
- `idade`: Idade da pessoa calculada em anos.
- `faixaIdade`: Faixa etária da pessoa.
- `signo`: Signo zodiacal da pessoa.
- `rg`: Número do RG da pessoa.
- `ufRg`: Unidade Federativa (UF) emissora do RG.
- `status`: Status do CPF na Receita Federal.
- `statusData`: Data da última atualização do status na Receita Federal.
- `maeNome`: Nome da mãe da pessoa.
- `obitoProvavel`: Indicação de possível óbito.
- `tituloEleitoral`: Número do título de eleitor.
- `grauInstrucao`: Grau de instrução da pessoa.
- `dependentes`: Número de dependentes.
- `estadoCivil`: Estado civil da pessoa.
- `renda`: Renda mensal estimada.
- `ppe`: Indicação de Pessoa Politicamente Exposta (PPE).
- `pis`: Número do PIS.
- `telefonesMoveis`: Lista de números de telefone móvel.
- `enderecos`: Lista de endereços da pessoa.
- `emails`: Lista de endereços de email da pessoa.

**Campos Comuns para CNPJ e CPF:**

- `participacoesEmpresas`: Lista de participações em outras empresas.
- `quantidade_dividas`: Número total de dívidas.
- `valor_total_dividas`: Valor total das dívidas.
- `dividas`: Lista detalhada de dívidas.
- `historico_consultas`: Histórico de consultas realizadas.
- `ccf`: Informações sobre Cheques sem Fundo (CCF).
- `rfb`: Informações adicionais da Receita Federal.
- `protestos`: Registro de protestos.
- `processos`: Registro de processos judiciais.
- `veiculos`: Veja documentação nas seções abaixo (se solicitado via parâmetro `veiculos`).
- `pefin|refin`: Veja documentação nas seções **PEFIN/REFIN Serasa** e **PEFIN/REFIN Boa Vista** (se solicitado via parâmetro `pefin|refin`).
- `completed`: Indica que todas as consultas foram concluídas

# PEFIN/REFIN Serasa

**Estrutura Geral**

```json
{
  "msg": "",
  "status": "sucesso",
  "parametro": "02492851000124",
  "informacoes": [
    {
      "user": {
        "Razao_Social": "AXON OLEO & GAS COMERCIO DE PECAS SOBRESSALENTES LTDA",
        "CNPJ": "02492851000124",
        "Nire": "",
        "Data_da_Fundacao": "28/04/1998",
        "Insc._Estadual": "",
        "Situacao_CNPJ": "ATIVA",
        "Data": "03/07/2024",
        "Natureza_Juridica": "2062-SOCIEDADE EMPRESARIA LIMITADA",
        "Ramo_de_Atividade_Primario": "4663000-COMÉRCIO ATACADISTA DE MÁQUINAS E EQUIPAMENTOS PARA USO INDUSTRIAL; PARTES E PEÇAS"
      },
      "bello": [
        {
          "ocorrencia": "",
          "entrada": "",
          "vencimento": "10/04/2024",
          "valor": "4.069,28",
          "informante": "BASE I",
          "contrato": "4100177816",
          "avalista": "NAO",
          "cidade": "",
          "uf": "",
          "situacao": "",
          "credor": "PETROLEO BRASILEIRO S/A PETROBRAS",
          "orgaoemissor": "SerasaExperian-Pefin",
          "totalpendencias": "1",
          "totalcredores": "",
          "totalvalor": "4.069,28",
          "categoria": "PENDÊNCIAS FINANCEIRAS",
          "modalidade": "OUTRAS OPER"
        },
        {
          "ocorrencia": "",
          "entrada": "",
          "vencimento": "11/02/2024",
          "valor": "286.045,69",
          "informante": "BASE I",
          "contrato": "000615127",
          "avalista": "NAO",
          "cidade": "",
          "uf": "",
          "situacao": "",
          "credor": "PETROLEO BRASILEIRO S/A PETROBRAS",
          "orgaoemissor": "SerasaExperian-Pefin",
          "totalpendencias": "1",
          "totalcredores": "",
          "totalvalor": "286.045,69",
          "categoria": "PENDÊNCIAS FINANCEIRAS",
          "modalidade": "OUTRAS OPER"
        }
      ],
      "valorTotalPendencias": 2449395.4,
      "total": 43,
      "valorTotalPendenciasFinanceiras": 2449395.4,
      "totalPendenciasFinanceiras": 43,
      "valorTotalPendenciasRefin": 0,
      "totalPendenciasRefin": 0,
      "valorTotalPendenciasVencidas": 0,
      "totalPendenciasVencidas": 0
    }
  ]
}
```

### Descrição dos Campos

| Campo         | Descrição                                                         | Tipo   | Observações                                     |
| :------------ | :---------------------------------------------------------------- | :----- | :---------------------------------------------- |
| `msg`         | Mensagem de status da consulta (geralmente vazia).                | String |                                                 |
| `status`      | Status da consulta (`"sucesso"` indica consulta bem-sucedida).    | String |                                                 |
| `parametro`   | Parâmetro de entrada da consulta (normalmente o CNPJ da empresa). | String |                                                 |
| `informacoes` | Array contendo informações detalhadas sobre a empresa consultada. | Array  | Cada elemento é um objeto com dados da empresa. |

### Estrutura "informacoes" (Objeto da Empresa)

Cada objeto dentro do array `informacoes` contém os seguintes campos:

#### Dados Cadastrais (`user`)

| Campo                        | Descrição                                                                                        | Tipo   | Observações |
| :--------------------------- | :----------------------------------------------------------------------------------------------- | :----- | :---------- |
| `Razao_Social`               | Razão social da empresa.                                                                         | String |             |
| `CNPJ`                       | CNPJ da empresa.                                                                                 | String |             |
| `Nire`                       | Número de Identificação do Registro de Empresas (geralmente vazio para empresas limitadas).      | String |             |
| `Data_da_Fundacao`           | Data de fundação da empresa (formato: "DD/MM/AAAA").                                             | String |             |
| `Insc._Estadual`             | Inscrição Estadual da empresa (pode estar vazia dependendo da atividade).                        | String |             |
| `Situacao_CNPJ`              | Situação cadastral do CNPJ na Receita Federal ("ATIVA", "INAPTA", etc.).                         | String |             |
| `Data`                       | Data da consulta (formato: "DD/MM/AAAA").                                                        | String |             |
| `Natureza_Juridica`          | Código e descrição da natureza jurídica da empresa (ex: "2062 - SOCIEDADE EMPRESARIA LIMITADA"). | String |             |
| `Ramo_de_Atividade_Primario` | Código e descrição do ramo de atividade principal da empresa (CNAE).                             | String |             |

#### Pendências Financeiras (`bello`)

| Campo             | Descrição                                                                                            | Tipo   | Observações                                                       |
| :---------------- | :--------------------------------------------------------------------------------------------------- | :----- | :---------------------------------------------------------------- |
| `ocorrencia`      | Número da ocorrência (geralmente vazio).                                                             | String |                                                                   |
| `entrada`         | Data de entrada da pendência (geralmente vazio).                                                     | String |                                                                   |
| `vencimento`      | Data de vencimento da pendência (formato: "DD/MM/AAAA").                                             | String |                                                                   |
| `valor`           | Valor da pendência (formato: "X.XXX,XX", com ponto separando milhares e vírgula separando decimais). | String |                                                                   |
| `informante`      | Fonte da informação sobre a pendência (ex: "BASE I").                                                | String |                                                                   |
| `contrato`        | Número do contrato relacionado à pendência.                                                          | String |                                                                   |
| `avalista`        | Indica se há avalista na pendência ("SIM" ou "NAO").                                                 | String |                                                                   |
| `cidade`          | Cidade do credor (geralmente vazio).                                                                 | String |                                                                   |
| `uf`              | Estado do credor (geralmente vazio).                                                                 | String |                                                                   |
| `situacao`        | Situação da pendência (geralmente vazio).                                                            | String |                                                                   |
| `credor`          | Nome do credor.                                                                                      | String |                                                                   |
| `orgaoemissor`    | Órgão emissor da informação sobre a pendência.                                                       | String |                                                                   |
| `totalpendencias` | Total de pendências com o mesmo credor (normalmente "1").                                            | String |                                                                   |
| `totalcredores`   | Total de credores da empresa (pode estar vazio).                                                     | String |                                                                   |
| `totalvalor`      | Valor total das pendências com o mesmo credor (igual ao `valor` se houver apenas uma pendência).     | String | Formato: "X.XXX,XX" (ponto para milhares, vírgula para decimais). |
| `categoria`       | Categoria da pendência ("PENDÊNCIAS FINANCEIRAS", "DÍVIDAS VENCIDAS", "RESTRIÇÕES FINANCEIRAS").     | String |                                                                   |
| `modalidade`      | Modalidade da pendência ("OUTRAS OPER").                                                             | String |                                                                   |

#### Resumo das Pendências

| Campo                             | Descrição                                                                        | Tipo   | Observações |
| :-------------------------------- | :------------------------------------------------------------------------------- | :----- | :---------- |
| `valorTotalPendencias`            | Valor total de todas as pendências da empresa (formato: "X.XXX,XX").             | Number |             |
| `total`                           | Número total de pendências da empresa.                                           | Number |             |
| `valorTotalPendenciasFinanceiras` | Valor total das pendências financeiras (formato: "X.XXX,XX").                    | Number |             |
| `totalPendenciasFinanceiras`      | Número total de pendências financeiras.                                          | Number |             |
| `valorTotalPendenciasRefin`       | Valor total das pendências relacionadas a refinanciamento (formato: "X.XXX,XX"). | Number |             |
| `totalPendenciasRefin`            | Número total de pendências relacionadas a refinanciamento.                       | Number |             |
| `valorTotalPendenciasVencidas`    | Valor total das pendências vencidas (formato: "X.XXX,XX").                       | Number |             |
| `totalPendenciasVencidas`         | Número total de pendências vencidas.                                             | Number |             |

# PEFIN/REFIN Boa Vista

**Estrutura Geral**

```json
{
  "dadosCadastrais": [
    {
      "CpfCnpj": "02492851000124",
      "Protocolo": "",
      "NomeRazao": "AXON OLEO & GAS",
      "NomeFantasia": "AXON OLEO & GAS COMERCIO DE PECAS SOBRESSALENTES LTDA",
      "NascimentoFundacao": "28/04/1998",
      "Idade": "28/04/1998",
      "Sexo": "",
      "Signo": "",
      "NomeMae": "",
      "NomePai": "",
      "Rg": "",
      "OrigemCpf": "",
      "DataSituacaoCadastral": "03/07/2024",
      "SituacaoCadastral": "ATIVO",
      "CapitalSocial": null,
      "NaturezaJuridica": "",
      "AtividadeEconomicaPrincipal": "",
      "AtividadeEconomicaSecundaria": null,
      "Endereco": null,
      "Numero": null,
      "Complemento": null,
      "Bairro": null,
      "Cidade": null,
      "Uf": null,
      "Cep": null,
      "DataConsulta": "03/07/2024 03:36:01"
    }
  ],
  "spc": [
    [
      {
        "NomeAssociado": "BANCO SANTANDER S/A",
        "Valor": "5356,12",
        "DataDeInclusao": "22/01/2024",
        "DataDoVencimento": "05/12/2023",
        "Entidade": "",
        "NumeroContrato": "MP385666000002791066",
        "CompradorFiadorAvalista": "COMPRADOR",
        "TelefoneAssociado": "",
        "CidadeAssociado": "",
        "UfAssociado": ""
      },
      {
        "NomeAssociado": "BANCO SANTANDER S/A",
        "Valor": "73934,2",
        "DataDeInclusao": "04/01/2024",
        "DataDoVencimento": "18/11/2023",
        "Entidade": "",
        "NumeroContrato": "UG385630000000285030",
        "CompradorFiadorAvalista": "COMPRADOR",
        "TelefoneAssociado": "",
        "CidadeAssociado": "",
        "UfAssociado": ""
      },
      {
        "NomeAssociado": "BANCO SANTANDER S/A",
        "Valor": "1584,41",
        "DataDeInclusao": "14/12/2023",
        "DataDoVencimento": "26/09/2023",
        "Entidade": "",
        "NumeroContrato": "DE03856130017181",
        "CompradorFiadorAvalista": "COMPRADOR",
        "TelefoneAssociado": "",
        "CidadeAssociado": "",
        "UfAssociado": ""
      }
    ]
  ]
}
```

### Campos Principais

| Campo               | Descrição                                                                                 | Tipo  | Observações                                        |
| :------------------ | :---------------------------------------------------------------------------------------- | :---- | :------------------------------------------------- |
| `dadosCadastrais`   | Array contendo informações cadastrais da empresa ou indivíduo.                            | Array | Contém um único objeto com os detalhes cadastrais. |
| `spc`               | Array de arrays, cada um contendo objetos que representam pendências financeiras no SPC.  | Array | Cada sub-array agrupa pendências do mesmo credor.  |
| `consultaRealizada` | Array possivelmente usado para armazenar informações sobre a consulta (vazio no exemplo). | Array |                                                    |

### Detalhes dos Campos em `dadosCadastrais`

| Campo                                                                | Descrição                                                                                                    | Tipo   | Observações                    |
| :------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------- | :----- | :----------------------------- |
| `CpfCnpj`                                                            | CPF (indivíduo) ou CNPJ (empresa).                                                                           | String |                                |
| `Protocolo`                                                          | Protocolo da consulta (pode estar vazio).                                                                    | String |                                |
| `NomeRazao`                                                          | Nome completo (indivíduo) ou Razão Social (empresa).                                                         | String |                                |
| `NomeFantasia`                                                       | Nome Fantasia da empresa (se houver).                                                                        | String |                                |
| `NascimentoFundacao`                                                 | Data de nascimento (indivíduo) ou fundação (empresa).                                                        | String | Formato: "DD/MM/AAAA"          |
| `Idade`                                                              | Idade do indivíduo ou tempo de existência da empresa (parece duplicar a informação de `NascimentoFundacao`). | String | Formato: "DD/MM/AAAA"          |
| `Sexo`                                                               | Sexo do indivíduo (M/F) ou vazio para empresas.                                                              | String |                                |
| `Signo`                                                              | Signo astrológico do indivíduo (se disponível) ou vazio para empresas.                                       | String |                                |
| `NomeMae`                                                            | Nome da mãe do indivíduo (se disponível) ou vazio para empresas.                                             | String |                                |
| `NomePai`                                                            | Nome do pai do indivíduo (se disponível) ou vazio para empresas.                                             | String |                                |
| `Rg`                                                                 | Número do RG do indivíduo (se disponível) ou vazio para empresas.                                            | String |                                |
| `OrigemCpf`                                                          | Origem do CPF (se disponível) ou vazio.                                                                      | String |                                |
| `DataSituacaoCadastral`                                              | Data da última atualização da situação cadastral.                                                            | String | Formato: "DD/MM/AAAA"          |
| `SituacaoCadastral`                                                  | Situação cadastral atual do CPF ou CNPJ (ex: "ATIVO", "INATIVO", "SUSPENSO").                                | String |                                |
| `CapitalSocial`                                                      | Capital social da empresa (valor numérico) ou nulo para indivíduos.                                          | Number |                                |
| `NaturezaJuridica`                                                   | Natureza jurídica da empresa (ex: "Sociedade Empresária Limitada") ou vazio para indivíduos.                 | String |                                |
| `AtividadeEconomicaPrincipal`                                        | Código da atividade econômica principal da empresa (CNAE) ou vazio para indivíduos.                          | String |                                |
| `AtividadeEconomicaSecundaria`                                       | Array de códigos de atividades econômicas secundárias da empresa (CNAE) ou nulo para indivíduos.             | Array  |                                |
| `Endereco`, `Numero`, `Complemento`, `Bairro`, `Cidade`, `Uf`, `Cep` | Informações de endereço (podem estar vazias).                                                                | String |                                |
| `DataConsulta`                                                       | Data e hora em que a consulta foi realizada.                                                                 | String | Formato: "DD/MM/AAAA HH:MM:SS" |

### Detalhes dos Campos em `spc`

| Campo                     | Descrição                                                                     | Tipo   | Observações                                                  |
| :------------------------ | :---------------------------------------------------------------------------- | :----- | :----------------------------------------------------------- |
| `NomeAssociado`           | Nome da empresa credora que registrou a pendência.                            | String |                                                              |
| `Valor`                   | Valor da pendência.                                                           | String | Formato: "XXXX,XX" (utilizar vírgula como separador decimal) |
| `DataDeInclusao`          | Data de inclusão da pendência no SPC.                                         | String | Formato: "DD/MM/AAAA"                                        |
| `DataDoVencimento`        | Data original de vencimento da dívida.                                        | String | Formato: "DD/MM/AAAA"                                        |
| `Entidade`                | Entidade relacionada à pendência (pode estar vazio).                          | String |                                                              |
| `NumeroContrato`          | Número do contrato relacionado à pendência.                                   | String |                                                              |
| `CompradorFiadorAvalista` | Indica o papel do devedor na pendência ("COMPRADOR", "FIADOR" ou "AVALISTA"). | String |                                                              |
| `TelefoneAssociado`       | Telefone da empresa credora (pode estar vazio).                               | String |                                                              |
| `CidadeAssociado`         | Cidade da empresa credora (pode estar vazio).                                 | String |                                                              |
| `UfAssociado`             | Estado da empresa credora (pode estar vazio).                                 | String |                                                              |

# Consulta Veículos

## Estrutura JSON de Veículos

Este JSON contém informações sobre um ou mais veículos.

```json
[
  {
    "placa": "PYI0623",
    "municipio": "Teresina",
    "uf": "PI",
    "renavam": "1097352517",
    "chassi": "9BHBG41DAHP651525",
    "motor": "F4FAGU189152",
    "ano_fabricacao": "2016",
    "ano_modelo": "2017",
    "marca_modelo": "HYUNDAI/HB20S 1.6M COMF",
    "procedencia": "",
    "especie": "",
    "combustivel": "ALCOOL/GASOLINA",
    "cor": "BRANCA"
  }
]
```

### Descrição dos Campos

| Campo            | Descrição                                      | Tipo   | Observações                    |
| :--------------- | :--------------------------------------------- | :----- | :----------------------------- |
| `veiculos`       | Array contendo objetos com dados dos veículos. | Array  |                                |
| `placa`          | Placa do veículo.                              | String |                                |
| `municipio`      | Município de registro do veículo.              | String |                                |
| `uf`             | Estado de registro do veículo (sigla).         | String |                                |
| `renavam`        | Número do RENAVAM.                             | String |                                |
| `chassi`         | Número do chassi.                              | String |                                |
| `motor`          | Número do motor.                               | String |                                |
| `ano_fabricacao` | Ano de fabricação.                             | String | Formato numérico (ex: "2016"). |
| `ano_modelo`     | Ano do modelo.                                 | String | Formato numérico (ex: "2017"). |
| `marca_modelo`   | Marca e modelo do veículo.                     | String |                                |
| `procedencia`    | Procedência do veículo.                        | String | Pode estar vazio.              |
| `especie`        | Espécie do veículo.                            | String | Pode estar vazio.              |
| `combustivel`    | Tipo de combustível.                           | String |                                |
| `cor`            | Cor do veículo.                                | String |                                |

### Observações

- Alguns campos podem estar vazios ou nulos, dependendo da disponibilidade de informações.
- A estrutura da resposta pode sofrer alterações futuras. Consulte a documentação oficial do CreditHub para obter informações atualizadas.

Para mais detalhes e atualizações, consulte a [documentação oficial do CreditHub](https://github.com/iCheques/api-simples-doc/blob/main/README.md).
