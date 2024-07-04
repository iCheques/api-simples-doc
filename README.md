## Documentação da API de Consulta Simples JSON PJ/PF CreditHub

A API de Consulta de CNPJ/CPF CreditHub permite obter informações detalhadas sobre empresas brasileiras a partir de seu número de CNPJ e de pessoas físicas a partir do seu CPF.

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
- `Veiculos` (opcional): Se definido como `true`, retorna informações sobre veículos.

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
fetch('https://irql.credithub.com.br/simples/abcdef12345/08075274000402', {
  method: 'GET',
  headers: {
    'Content-Type': 'application/json'
  }
})
.then(response => {
  if (!response.ok) {
    throw new Error('Erro na requisição');
  }
  return response.json();
})
.then(data => {
  console.log(data); // Dados da consulta em JSON
})
.catch(error => {
  console.error('Erro:', error);
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

### Observações

- Alguns campos podem estar vazios ou nulos, dependendo da disponibilidade de informações.
- A estrutura da resposta pode sofrer alterações futuras. Consulte a documentação oficial do CreditHub para obter informações atualizadas.

### Parâmetros GET Adicionais

- `REFIN` (opcional): Se definido como `true`, retorna informações de Refin.
- `PEFIN` (opcional): Se definido como `true`, retorna informações de Pefin.
- `Veiculos` (opcional): Se definido como `true`, retorna informações sobre veículos.

Para mais detalhes e atualizações, consulte a [documentação oficial do CreditHub](https://github.com/iCheques/api-simples-doc/blob/main/README.md).
