# Análise Defensiva de CORS — OWASP Juice Shop

## 1. Objetivo

Investigar os cabeçalhos HTTP relacionados ao Cross-Origin Resource Sharing (CORS) em uma instância do OWASP Juice Shop executada em laboratório local.

## 2. Ambiente

- **Sistema de análise:** Kali Linux
- **Aplicação-alvo:** OWASP Juice Shop
- **Endereço local:** `http://192.168.115.5:3000`
- **Ferramentas:** Firefox Developer Tools (Network) e curl

## 3. Endpoint analisado

- **Método HTTP:** GET
- **Caminho:** `/api/Challenges/?name=Score`
- **Recurso observado:** informações relacionadas ao desafio Score Board

## 4. Evidências observadas na resposta GET

- **Status HTTP:** `304 Not Modified`
- **`Access-Control-Allow-Origin`:** `*`
- **`Access-Control-Allow-Credentials`:** não identificado na resposta examinada
- **`Vary`:** `Accept-Encoding`
- **Conteúdo observado:** resposta relacionada ao desafio Score Board
- **Dados pessoais ou privados:** não foram identificados no conteúdo observado

## 5. Análise da resposta GET

O cabeçalho `Access-Control-Allow-Origin: *` permite que navegadores autorizem páginas de diferentes origens a ler determinadas respostas, respeitando as regras de CORS. Essa configuração não permite, por si só, que uma página leia respostas que exijam credenciais com cookies quando a resposta não autoriza credenciais por CORS.

O conteúdo observado estava relacionado a informações do desafio Score Board. A presença do curinga `*`, isoladamente, não comprova uma vulnerabilidade. O cabeçalho `Vary: Accept-Encoding` está relacionado à negociação da codificação da resposta e não demonstra, por si só, uma falha de CORS.

O status `304 Not Modified` indica que o recurso não precisou ser retransmitido devido à validação de cache. A análise do conteúdo deve considerar também as respostas e os cabeçalhos efetivamente observados nas ferramentas utilizadas.

## 6. Teste da requisição OPTIONS

- **Endpoint:** `/api/Challenges/?name=Score`
- **Método HTTP:** `OPTIONS`
- **Status observado:** `204 No Content`
- **`Access-Control-Allow-Origin`:** `*`
- **`Access-Control-Allow-Methods`:** métodos anunciados pela resposta, incluindo `GET`, `HEAD`, `PUT`, `PATCH`, `POST` e `DELETE`
- **`Content-Length`:** `0`
- **`Access-Control-Allow-Credentials`:** não identificado na resposta examinada

### Análise da resposta OPTIONS

A resposta `204 No Content` indica que o servidor respondeu à requisição sem conteúdo no corpo. A lista de métodos anunciados em `Access-Control-Allow-Methods` não comprova, isoladamente, que todos esses métodos funcionem no endpoint nem que operações estejam disponíveis sem autenticação ou autorização.

A configuração `Access-Control-Allow-Origin: *` deve ser avaliada considerando a finalidade do recurso, a sensibilidade dos dados e os controles de acesso aplicáveis.

Não foram testadas operações de alteração ou exclusão de dados nesta análise.

## 7. Verificação de Access-Control-Allow-Credentials

Foi examinada a resposta OPTIONS do endpoint `/api/Challenges/?name=Score`.

**Resultado:** o cabeçalho `Access-Control-Allow-Credentials` não foi encontrado na resposta examinada.

Essa observação se limita à resposta analisada. A ausência do cabeçalho nessa resposta não comprova que todas as respostas da aplicação tenham a mesma configuração nem que não existam outros riscos.

## 8. Conclusão

Até o momento, não foi confirmada uma vulnerabilidade de CORS com base nas evidências coletadas. A análise foi observacional e limitada aos cabeçalhos e ao conteúdo disponibilizado pelo endpoint no laboratório local.

Uma avaliação adicional exigiria evidências que demonstrassem impacto real, considerando as origens envolvidas, a natureza dos dados, o uso de credenciais e os controles de autenticação e autorização.

## 9. Escopo e uso responsável

Todos os testes foram realizados exclusivamente no laboratório local autorizado. A análise não teve como objetivo acessar dados de terceiros nem alterar ou excluir dados da aplicação.
