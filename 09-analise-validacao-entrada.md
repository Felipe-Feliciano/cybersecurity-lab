# Análise de validação de entrada — OWASP Juice Shop

## 1. Objetivo

Observar como a API de pesquisa de produtos reage a uma entrada contendo caracteres especiais e uma expressão semelhante a SQL.

## 2. Ambiente

- Cliente: Kali Linux
- Aplicação: OWASP Juice Shop
- Alvo: 192.168.115.5:3000
- Ferramenta: curl
- Método HTTP: GET
- Endpoint: `/rest/products/search`

## 3. Teste realizado

Parâmetro enviado:

`q=' OR 1=1 --`

Resultado observado:

- Status HTTP: `500 Internal Server Error`
- Mensagem: `SQLITE_ERROR: incomplete input`
- Tipo de resposta: `text/html; charset=utf-8`

Evidência: `07-evidencias/erro-validacao-api.txt`

## 4. Análise técnica

A entrada testada provocou um erro interno na aplicação. A mensagem retornada menciona o SQLite e indica que uma operação envolvendo SQL não foi processada corretamente.

A resposta também expõe detalhes técnicos do erro ao cliente, em vez de apresentar uma mensagem genérica.

Esse comportamento merece investigação. Entretanto, o teste isolado não comprova que seja possível executar uma SQL injection com sucesso ou acessar dados não autorizados.

## 5. Impacto potencial

- Exposição de detalhes internos da implementação.
- Tratamento inadequado de entradas inesperadas.
- Possibilidade de existência de falhas na construção ou no tratamento de consultas ao banco de dados, a confirmar por testes adicionais.

## 6. Recomendações

- Utilizar consultas parametrizadas (*prepared statements*).
- Validar e tratar entradas de acordo com o formato esperado.
- Retornar mensagens genéricas de erro ao cliente.
- Manter detalhes técnicos nos logs internos, com acesso restrito.
- Realizar testes adicionais controlados para confirmar a causa do erro.

## 7. Conclusão

A entrada testada provocou uma resposta HTTP 500 com a mensagem `SQLITE_ERROR: incomplete input`.

O resultado indica um erro de processamento e exposição de detalhes técnicos. Não foi comprovada, com este teste isolado, uma SQL injection explorável.

Teste realizado em laboratório local para fins educacionais.
