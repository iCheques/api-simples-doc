# FAQ - CALLBACK e Consultas

## O que acontece se eu realizar a primeira consulta sem o parâmetro CALLBACK?

Se a primeira consulta for enviada sem o parâmetro CALLBACK, todas as consultas subsequentes realizadas no mesmo dia, para o mesmo CPF ou CNPJ, **não retornarão no ENDPOINT**. O retorno estará disponível apenas no JSON recebido via HTTP.

Para verificar se o CALLBACK foi ignorado (evitando uma nova tarifação), consulte o campo `.config.callback` no JSON. Caso esse campo esteja vazio, significa que o CALLBACK não foi enviado na primeira consulta.

---

## Como posso testar o CALLBACK em ambiente de desenvolvimento?

Recomendamos o uso de ferramentas como [webhook.site](https://webhook.site) para testar o funcionamento do CALLBACK. Essa ferramenta permite monitorar o envio das notificações e validar se o ENDPOINT está configurado corretamente.
