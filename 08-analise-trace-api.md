# Análise do método HTTP TRACE — OWASP Juice Shop

## 1. Objetivo

Verificar o comportamento do servidor ao receber requisições HTTP com o método TRACE no laboratório local OWASP Juice Shop.

## 2. Ambiente de teste

- Cliente: Kali Linux
- Aplicação: OWASP Juice Shop
- Endereço: `192.168.115.5:3000`
- Ferramenta: curl

## 3. Testes realizados

### Teste 1 — Página inicial

- Método: TRACE
- Endpoint: `/`
- Resultado: `HTTP/1.1 200 OK`
- Observação: o servidor retornou o HTML da página inicial. Não foi observada reflexão dos cabeçalhos da requisição na resposta.

### Teste 2 — Endpoint da API

- Método: TRACE
- Endpoint: `/rest/products/search?q=apple`
- Resultado: `HTTP/1.1 500 Internal Server Error`
- Evidência: `07-evidencias/trace-api.txt`
- Observação: a resposta apresentou uma mensagem de erro, caminhos internos da aplicação e detalhes da pilha de execução (*stack trace*).

## 4. Análise de segurança

O segundo teste demonstrou que uma requisição TRACE para o endpoint informado provocou uma resposta HTTP 500 com detalhes internos da aplicação.

A exposição desses detalhes pode facilitar o reconhecimento da estrutura interna do sistema. Entretanto, os resultados não demonstram, isoladamente, exploração bem-sucedida ou comprometimento da aplicação.

## 5. Impacto potencial

- Divulgação de caminhos e componentes internos.
- Maior facilidade para reconhecimento técnico da aplicação.
- Exposição desnecessária de detalhes de implementação em mensagens de erro.

## 6. Recomendações

- Desabilitar o método TRACE se ele não for necessário.
- Retornar mensagens de erro genéricas aos clientes.
- Registrar detalhes técnicos apenas nos logs internos, com acesso restrito.
- Revisar o tratamento de métodos HTTP não esperados.
- Repetir os testes após a aplicação das correções.

## 7. Conclusão

Foi observada uma resposta 200 OK para TRACE na página inicial e uma resposta 500 Internal Server Error no endpoint testado, com exposição de detalhes internos.

O comportamento merece investigação e correção no ambiente de laboratório. Não foi confirmada, com esses testes isolados, uma vulnerabilidade explorável.

## 8. Evidência

Arquivo: `07-evidencias/trace-api.txt`
