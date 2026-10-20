# Análise de Cabeçalhos de Segurança HTTP — OWASP Juice Shop

## 1. Objetivo
Verificar os cabeçalhos de segurança HTTP da página principal do OWASP Juice Shop em um laboratório local.

## 2. Ambiente
- Sistema de análise: Kali Linux
- Aplicação: OWASP Juice Shop
- Endereço: `http://192.168.115.5:3000`
- Ferramenta: curl

## 3. Método
Foi realizada uma consulta HTTP HEAD com `curl -s -I` para observar os cabeçalhos da resposta. Também foi consultado o HTML retornado para procurar uma possível política CSP.

## 4. Resultados observados

- Status HTTP: `200 OK`
- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: SAMEORIGIN`
- `Feature-Policy: payment 'self'`
- `Access-Control-Allow-Origin: *`
- `Vary: Accept-Encoding`
- `Content-Security-Policy`: não encontrado nos cabeçalhos HTTP verificados
- CSP no HTML consultado: não encontrada pela busca realizada

## 5. Análise defensiva

O cabeçalho `X-Content-Type-Options: nosniff` ajuda a reduzir riscos associados à interpretação incorreta de tipos MIME.

O cabeçalho `X-Frame-Options: SAMEORIGIN` restringe o enquadramento da página por sites de outras origens e ajuda a mitigar ataques de clickjacking.

O cabeçalho `Feature-Policy` é uma versão antiga da política de permissões, sucedida por `Permissions-Policy`.

Não foi encontrada uma política `Content-Security-Policy` nos cabeçalhos HTTP da página principal nem na busca realizada no HTML. Uma CSP bem planejada pode reduzir certos riscos de injeção de conteúdo, mas sua ausência, isoladamente, não comprova uma vulnerabilidade explorável.

O cabeçalho `Access-Control-Allow-Origin: *` também não comprova, por si só, uma falha de CORS.

## 6. Conclusão provisória

Foram identificados cabeçalhos de segurança presentes e uma possível oportunidade de melhoria relacionada à política CSP. A conclusão está limitada à página principal e às verificações realizadas; outras rotas e respostas podem ter configurações diferentes.

## 7. Escopo

Análise observacional realizada exclusivamente no laboratório local autorizado. Nenhuma alteração foi feita na aplicação.

## Verificação adicional de cabeçalhos de segurança

### Referrer-Policy
- **Resultado:** não encontrado na resposta HTTP da página principal.
- **Análise:** a ausência pode permitir que o navegador utilize sua política padrão para o envio do cabeçalho `Referer`.
- **Recomendação:** avaliar a configuração explícita de uma política adequada.

### Permissions-Policy
- **Resultado:** não encontrado na resposta HTTP da página principal.
- **Análise:** não foi identificada uma política explícita de restrição de recursos do navegador nessa resposta.
- **Recomendação:** avaliar restrições para recursos como câmera, microfone e geolocalização, conforme a necessidade da aplicação.

### Observação
Os resultados representam verificações da resposta da página principal do laboratório. A ausência desses cabeçalhos não comprova, isoladamente, uma vulnerabilidade explorável.

### Strict-Transport-Security (HSTS)
- **Resultado:** não encontrado na resposta consultada via HTTP.
- **Análise:** o teste foi realizado com `http://192.168.115.5:3000`. O HSTS é uma política recebida por HTTPS, portanto sua ausência nessa resposta HTTP não foi classificada como vulnerabilidade.
- **Recomendação:** se a aplicação for disponibilizada por HTTPS em um ambiente apropriado, avaliar a configuração de HSTS conforme a arquitetura e os requisitos de segurança.

Verificação do método HTTP TRACE

Método testado: TRACE

Resposta HTTP observada: 200 OK

Comportamento: o servidor retornou o HTML da página principal do OWASP Juice Shop.

Verificação de reflexão: o marcador X-Lab-Test: trace-check não foi encontrado no corpo da resposta.

Conclusão: os resultados obtidos não confirmam uma vulnerabilidade de reflexão via TRACE, nem permitem afirmar que o método esteja desabilitado. São necessárias verificações adicionais para determinar o comportamento exato do servidor.
