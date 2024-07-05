## Documentação da API de Consulta Simples JSON PJ/PF CreditHub 🚀

A API de Consulta de CNPJ/CPF CreditHub é a ferramenta ideal para obter informações detalhadas e atualizadas sobre empresas (CNPJ) e pessoas físicas (CPF) no Brasil. 💼 Com ela, você pode integrar dados relevantes aos seus sistemas e tomar decisões mais assertivas. 🎯

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

Qualquer pessoa ou empresa que precise de informações sobre CNPJs ou CPFs pode usar a API CreditHub. Desenvolvedores, analistas de crédito, profissionais de marketing e compliance são alguns exemplos de usuários que podem se beneficiar desta ferramenta. 👨‍💻👩‍💼

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

- `refin` (opcional): Se definido como `true`, retorna informações de Refin.
- `pefin` (opcional): Se definido como `true`, retorna informações de Pefin.
- `veiculos` (opcional): Se definido como `true`, retorna informações sobre veículos.
- `callback` (opcional): A URL que irá receber os JSONs com os dados atualizados.

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
- `veiculos`: Lista de veículos associados, contendo:
  - `placa`: Placa do veículo.
  - `renavam`: Número do Renavam.
  - `chassi`: Número do chassi.
  - `marca`: Marca do veículo.
  - `modelo`: Modelo do veículo.
  - `anoFabricacao`: Ano de fabricação do veículo.
  - `anoModelo`: Ano do modelo do veículo.
  - `cor`: Cor do veículo.
  - `tipo`: Tipo do veículo (carro, moto, caminhão, etc.).
 
# Consulta Serasa (Pefin)

**Estrutura Geral**

```json
{
    "msg": "",
    "status": "sucesso",
    "parametro": "02492851000124",
    "informacoes": [{
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
        "bello": [{
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
         }],
        "valorTotalPendencias": 2449395.40,
        "total": 43,
        "valorTotalPendenciasFinanceiras": 2449395.40,
        "totalPendenciasFinanceiras": 43,
        "valorTotalPendenciasRefin": 0,
        "totalPendenciasRefin": 0,
        "valorTotalPendenciasVencidas": 0,
        "totalPendenciasVencidas": 0
    }]
}
```


**Campos**

- **msg:** Mensagem de status da consulta (geralmente vazia).
- **status:** Status da consulta ("sucesso" indica que a consulta foi realizada com sucesso).
- **parametro:** Parâmetro de entrada da consulta (neste caso, o CNPJ da empresa).
- **informacoes:** Array contendo informações sobre a empresa consultada.

**Estrutura "user"**

- **Razao\_Social:** Razão social da empresa.
- **CNPJ:** CNPJ da empresa.
- **Nire:** Número de Identificação do Registro de Empresas (geralmente vazio para empresas limitadas).
- **Data\_da\_Fundacao:** Data de fundação da empresa.
- **Insc.\_Estadual:** Inscrição Estadual (pode estar vazia dependendo da atividade da empresa).
- **Situacao\_CNPJ:** Situação cadastral do CNPJ na Receita Federal ("ATIVA" indica que o CNPJ está ativo).
- **Data:** Data da consulta.
- **Natureza\_Juridica:** Código e descrição da natureza jurídica da empresa.
- **Ramo\_de\_Atividade\_Primario:** Código e descrição do ramo de atividade principal da empresa.

**Estrutura "bello"**

Contém um array de objetos, cada um representando uma pendência financeira da empresa.

- **ocorrencia:** Número da ocorrência (geralmente vazio).
- **entrada:** Data de entrada da pendência (geralmente vazio).
- **vencimento:** Data de vencimento da pendência.
- **valor:** Valor da pendência.
- **informante:** Fonte da informação sobre a pendência.
- **contrato:** Número do contrato relacionado à pendência.
- **avalista:** Indica se há avalista na pendência ("NAO" indica que não há).
- **cidade:** Cidade do credor (geralmente vazio).
- **uf:** Estado do credor (geralmente vazio).
- **situacao:** Situação da pendência (geralmente vazio).
- **credor:** Nome do credor.
- **orgaoemissor:** Órgão emissor da informação sobre a pendência.
- **totalpendencias:** Total de pendências com o mesmo credor 
- **totalcredores:** Total de credores da empresa 
- **totalvalor:** Valor total das pendências com o mesmo credor 
- **categoria:** Categoria da pendência ("PENDÊNCIAS FINANCEIRAS", “DÍVIDAS VENCIDAS”, “RESTRIÇÕES FINANCEIRAS”).
- **modalidade:** Modalidade da pendência ("OUTRAS OPER").

**Resumo das Pendências**

- **valorTotalPendencias:** Valor total de todas as pendências da empresa.
- **total:** Número total de pendências da empresa.
- **valorTotalPendenciasFinanceiras:** Valor total das pendências financeiras.
- **totalPendenciasFinanceiras:** Número total de pendências financeiras.
- **valorTotalPendenciasRefin:** Valor total das pendências relacionadas a refinanciamento 
- **totalPendenciasRefin:** Número total de pendências relacionadas a refinanciamento 
- **valorTotalPendenciasVencidas:** Valor total das pendências vencidas 
- **totalPendenciasVencidas:** Número total de pendências vencidas 

# Consulta Boa Vista (Refin)
**Estrutura Geral**

```json
{
    "dadosCadastrais": [{
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
    }],
    "spc": [
        [{
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

**Campos:**

- **dadosCadastrais:** Array contendo um objeto com informações cadastrais da empresa ou indivíduo.
- **spc:** Array contendo arrays de objetos, cada um representando uma pendência financeira no SPC.
- **consultaRealizada:** Array que parece estar vazio neste exemplo, possivelmente usado para armazenar informações sobre a consulta.

**Estrutura "dadosCadastrais"**

- **CpfCnpj:** CPF ou CNPJ da empresa ou indivíduo.
- **Protocolo:** Protocolo da consulta (vazio neste caso).
- **NomeRazao:** Nome ou razão social da empresa ou indivíduo.
- **NomeFantasia:** Nome fantasia da empresa (se aplicável).
- **NascimentoFundacao:** Data de nascimento (para indivíduos) ou fundação (para empresas).
- **Idade:** Idade da empresa ou indivíduo (parece duplicar a informação de NascimentoFundacao).
- **Sexo:** Sexo do indivíduo (vazio para empresas).
- **Signo:** Signo astrológico do indivíduo (vazio para empresas).
- **NomeMae:** Nome da mãe do indivíduo (vazio para empresas).
- **NomePai:** Nome do pai do indivíduo (vazio para empresas).
- **Rg:** Número do RG do indivíduo (vazio para empresas).
- **OrigemCpf:** Origem do CPF (vazio neste caso).
- **DataSituacaoCadastral:** Data da última atualização da situação cadastral.
- **SituacaoCadastral:** Situação cadastral atual do CPF ou CNPJ (por exemplo, "ATIVO").
- **CapitalSocial:** Capital social da empresa (nulo para indivíduos).
- **NaturezaJuridica:** Natureza jurídica da empresa (vazio neste caso).
- **AtividadeEconomicaPrincipal:** Atividade econômica principal da empresa (vazio neste caso).
- **AtividadeEconomicaSecundaria:** Atividades econômicas secundárias da empresa.
- **Endereco:** Endereço da empresa ou indivíduo.
- **Numero:** Número do endereço.
- **Complemento:** Complemento do endereço.
- **Bairro:** Bairro do endereço.
- **Cidade:** Cidade do endereço.
- **Uf:** Estado do endereço.
- **Cep:** CEP do endereço.
- **DataConsulta:** Data e hora da consulta.

**Estrutura "spc"**

Contém um array de arrays de objetos. Cada objeto representa uma pendência financeira no SPC.

- **NomeAssociado:** Nome da empresa associada ao SPC que registrou a pendência.
- **Valor:** Valor da pendência.
- **DataDeInclusao:** Data em que a pendência foi incluída no SPC.
- **DataDoVencimento:** Data de vencimento original da pendência.
- **Entidade:** Entidade relacionada à pendência .
- **NumeroContrato:** Número do contrato relacionado à pendência.
- **CompradorFiadorAvalista:** Indica se o devedor é comprador, fiador ou avalista na pendência.
- **TelefoneAssociado:** Telefone da empresa associada.
- **CidadeAssociado:** Cidade da empresa associada 
- **UfAssociado:** Estado da empresa associada




### Observações

- Alguns campos podem estar vazios ou nulos, dependendo da disponibilidade de informações.
- A estrutura da resposta pode sofrer alterações futuras. Consulte a documentação oficial do CreditHub para obter informações atualizadas.

Para mais detalhes e atualizações, consulte a [documentação oficial do CreditHub](https://github.com/iCheques/api-simples-doc/blob/main/README.md).
