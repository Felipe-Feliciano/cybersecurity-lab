# Análise do Método HTTP TRACE

## 1. Objetivo

Verificar como a aplicação OWASP Juice Shop, executada em um laboratório local autorizado, responde a uma solicitação HTTP utilizando o método TRACE.

## 2. Ambiente de teste

- Aplicação: OWASP Juice Shop
- Sistema de teste: Kali Linux
- Servidor de destino: UbuntuCyberLab
- Endereço local: `192.168.115.5:3000`
- Endpoint testado: `/rest/products/search?q=apple`
- Método HTTP: TRACE

## 3. Resultado observado

- Status HTTP: `500 Internal Server Error`
- Tipo de conteúdo: `text/html; charset=utf-8`
- Mensagem apresentada: `Error: Unexpected path: /rest/products/search?q=apple`
- A resposta incluiu detalhes internos da aplicação, incluindo caminhos de arquivos e componentes do Express.

## 4. Análise de segurança

A resposta demonstra que a solicitação TRACE provocou um erro interno na aplicação. A página de erro expôs informações sobre a estrutura interna do software, incluindo caminhos de arquivos e trechos do stack trace.

Em ambientes de produção, essas informações devem ser ocultadas de respostas destinadas aos usuários, para reduzir a exposição de detalhes de implementação.

O resultado não confirma, por si só, uma vulnerabilidade explorável relacionada ao método TRACE. Não foi observado o retorno refletido da solicitação HTTP na resposta.

## 5. Cabeçalhos relevantes

- `Access-Control-Allow-Origin: *`
- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: SAMEORIGIN`
- `Content-Type: text/html; charset=utf-8`

## 6. Conclusão

O teste identificou uma resposta HTTP 500 com detalhes internos da aplicação. Recomenda-se verificar a configuração de tratamento de erros e evitar a exposição de stack traces em ambientes de produção.

O achado deve ser tratado como uma observação de segurança que requer investigação adicional, e não como uma vulnerabilidade confirmada.

## 7. Escopo

Todos os testes foram realizados na aplicação local do laboratório OWASP Juice Shop, em ambiente controlado e autorizado.
