# FAQ

### O que acontece se eu realizar a primeira consulta sem o parâmetro CALLBACK?
Se a primeira consulta for enviada sem o parâmetro CALLBACK, todas as consultas subsequentes realizadas no mesmo dia, para o mesmo CPF ou CNPJ, não retornarão no ENDPOINT. O retorno estará disponível apenas no JSON recebido via HTTP.

Para verificar se o CALLBACK foi ignorado (evitando uma nova tarifação), consulte o campo `.config.callback` no JSON. Caso esse campo esteja vazio, significa que o CALLBACK não foi enviado na primeira consulta.

### Como posso testar o CALLBACK em ambiente de desenvolvimento?
Recomendamos o uso de ferramentas como [webhook.site](https://webhook.site) para testar o funcionamento do CALLBACK. Essa ferramenta permite monitorar o envio das notificações e validar se o ENDPOINT está configurado corretamente.

### O CALLBACK será acionado somente no dia da consulta?
Sim, o CALLBACK será acionado apenas uma vez, no dia da consulta. Se você realizar novas consultas para o mesmo CPF ou CNPJ no mesmo dia, a resposta estará disponível diretamente no retorno HTTP, sem nova tarifação.

### Posso demorar para receber um CALLBACK?
Sim, pode haver atrasos no recebimento do CALLBACK. Isso pode ocorrer devido a fatores externos ao CreditHub, como atrasos nos tribunais, cartórios ou outros serviços integrados ao nosso HUB de crédito. No entanto, você receberá o resultado ao longo do dia, assim que a informação estiver disponível. 

Caso a informação não fique disponível no mesmo dia, ela não será retornada no dia seguinte. Será necessário realizar uma nova consulta.

### Como sei se uma consulta de uma informação não foi realizada?
Se uma informação não foi obtida, o JSON de retorno não terá o campo correspondente àquela informação. Isso indica que a consulta ainda está pendente ou não foi processada.

### Se o usuário desejar realizar a mesma consulta no mesmo dia, como proceder para evitar nova tarifação?
Basta realizar a consulta normalmente. Se for no mesmo dia e para o mesmo CPF ou CNPJ, o sistema retornará o resultado diretamente no JSON via HTTP, sem acionar o CALLBACK e sem aplicar uma nova tarifação.

### O que significa este trecho no retorno da consulta?

```json
"push": {
    "DatabaseFollowDocumentConsultaSimplesRFB": "674df921d49154afc00817d4",
    ...
}
```

#### Qual é o uso desses campos?

Esses campos são internos do CreditHub e são utilizados exclusivamente para depuração das consultas em nosso sistema. Eles ajudam nossa equipe a verificar o estado da consulta e identificar possíveis problemas.

Por exemplo, com base em um ID como 674df921d49154afc00817d4, podemos depurar utilizando nossa interface interna:

https://irql.credithub.com.br/?q=SELECT%20FROM%20%27PUSH%27.%27DELETEDJOB%27&id=674df921d49154afc00817d4&apiKey=********

Com isso, conseguimos saber qual máquina processou a consulta, entre outras informações técnicas.

#### Preciso me preocupar com esses campos?

Não. A Consulta Simples foi projetada para lidar com toda essa complexidade, para que você receba o resultado de maneira direta e eficiente. Não é necessário se preocupar com esses detalhes técnicos. Caso queira salvar esses IDs, você poderá ter um suporte técnico mais rápido em caso de falhas.

### O CALLBACK pode ser usado para forçar uma nova consulta no mesmo dia?
Não. A chave para identificar a consulta é composta pela **APIKEY**, o dia da consulta e o documento consultado. Mesmo que você envie um CALLBACK diferente, ele será ignorado. Apenas o primeiro CALLBACK enviado será considerado válido.
