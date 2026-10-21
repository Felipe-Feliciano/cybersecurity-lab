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

## 8. Testes complementares de pesquisa

### Busca por `juice`

- Método: GET
- Endpoint: `/rest/products/search?q=juice`
- Resultado: `200 OK`
- Resposta: lista de produtos relacionados ao termo pesquisado.
- Evidência: `07-evidencias/busca-juice.txt`

### Busca por valor numérico

- Método: GET
- Parâmetro: `q=12345`
- Resultado: `200 OK`
- Resposta: lista vazia (`data: []`).

### Busca sem correspondências

- Método: GET
- Parâmetro: `q=produto_inexistente_xyz`
- Resultado: `200 OK`
- Resposta: lista vazia (`data: []`).

Esses resultados são compatíveis com o comportamento normal da pesquisa. O teste com entrada especial que retornou erro SQLite continua sendo o principal ponto que merece investigação adicional.

## 9. Análise de tratamento de rotas inexistentes

### Objetivo

Verificar como a aplicação responde a uma requisição para uma rota de API inexistente e identificar possíveis exposições de informações técnicas.

### Teste realizado

- Método HTTP: GET
- Caminho solicitado: `/api/rota-inexistente-lab`
- Ambiente: Kali Linux e OWASP Juice Shop local
- Resultado: `500 Internal Server Error`
- Mensagem: `Unexpected path: /api/rota-inexistente-lab`
- Tecnologia identificada: Express 4.22.1
- Evidência: `07-evidencias/erro-rota-inexistente.txt`

### Análise técnica

A aplicação retornou um erro HTTP 500 para uma rota inexistente. A resposta incluiu um stack trace com caminhos internos de arquivos e informações sobre componentes da aplicação.

Esse comportamento expõe detalhes da implementação que, em um ambiente de produção, deveriam ser ocultados dos clientes.

### Impacto potencial

A exposição de informações técnicas pode facilitar o reconhecimento da arquitetura interna e auxiliar a investigação de outras falhas. O teste, isoladamente, não demonstra acesso não autorizado nem comprometimento de dados.

### Recomendações defensivas

- Configurar um tratamento centralizado de erros.
- Retornar mensagens genéricas para clientes em ambientes de produção.
- Desabilitar a exposição de stack traces nas respostas HTTP.
- Registrar detalhes técnicos somente em logs internos com acesso restrito.
- Retornar `404 Not Found` para rotas inexistentes, quando apropriado à arquitetura da aplicação.

### Conclusão

O teste identificou uma resposta HTTP 500 contendo informações sobre o Express, caminhos internos e stack trace. O resultado caracteriza exposição de informações técnicas no contexto do laboratório e reforça a importância de um tratamento adequado de erros.

O achado foi observado em um ambiente educacional local e não deve ser generalizado para outras aplicações sem testes adicionais.

## 10. Análise dos cabeçalhos HTTP em respostas de erro

### Objetivo

Verificar os cabeçalhos HTTP retornados pela aplicação quando uma rota inexistente provoca um erro interno.

### Resultado observado

- Status HTTP: `500 Internal Server Error`
- `Access-Control-Allow-Origin: *`
- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: SAMEORIGIN`
- `Content-Type: text/html; charset=utf-8`
- Evidência: `07-evidencias/cabecalhos-erro-rota.txt`

### Análise técnica

A resposta contém os cabeçalhos `X-Content-Type-Options: nosniff` e `X-Frame-Options: SAMEORIGIN`, que oferecem proteções específicas no navegador.

O cabeçalho `Access-Control-Allow-Origin: *` permite acesso por qualquer origem a recursos compatíveis com essa política CORS. Sua presença, isoladamente, não comprova uma vulnerabilidade; é necessário considerar os dados disponibilizados e o comportamento de autenticação da aplicação.

A resposta continua retornando o código HTTP 500. Em conjunto com o teste anterior, que revelou um stack trace, isso indica que o tratamento de erros merece atenção.

### Recomendações defensivas

- Manter os cabeçalhos de proteção adequados ao contexto da aplicação.
- Avaliar se a política CORS deve permitir todas as origens ou apenas origens autorizadas.
- Evitar a exposição de stack traces e caminhos internos em respostas enviadas aos clientes.
- Utilizar códigos HTTP apropriados para recursos inexistentes e erros internos.
- Testar as configurações em um ambiente controlado antes de aplicá-las em produção.

### Conclusão

Foram observados cabeçalhos de proteção na resposta de erro, mas também foi confirmado o status HTTP 500 para uma rota inexistente. Os resultados reforçam a necessidade de avaliar conjuntamente o tratamento de erros, a exposição de informações técnicas e a política CORS.

A presença de `Access-Control-Allow-Origin: *` não foi considerada, isoladamente, prova de falha de segurança.
