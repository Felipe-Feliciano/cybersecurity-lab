# Análise de Cabeçalhos de Segurança HTTP — OWASP Juice Shop

## 1. Objetivo

Verificar os cabeçalhos HTTP relacionados à segurança da página principal do OWASP Juice Shop em um laboratório local.

## 2. Ambiente

- **Sistema de análise:** Kali Linux
- **Aplicação-alvo:** OWASP Juice Shop
- **Endereço local:** `http://192.168.115.5:3000`
- **Ferramentas:** curl e Firefox

## 3. Metodologia

Foi realizada uma consulta HTTP com o método `HEAD` para observar os cabeçalhos da resposta. Também foi consultado o HTML retornado para procurar uma possível política Content Security Policy (CSP).

As verificações foram limitadas à página principal e às respostas consultadas durante a atividade.

## 4. Resultados observados

Na resposta HTTP examinada, foram observados:

- **Status HTTP:** `200 OK`
- **`X-Content-Type-Options`:** `nosniff`
- **`X-Frame-Options`:** `SAMEORIGIN`
- **`Feature-Policy`:** `payment 'self'`
- **`Access-Control-Allow-Origin`:** `*`
- **`Vary`:** `Accept-Encoding`
- **`Content-Security-Policy`:** não encontrado nos cabeçalhos HTTP verificados
- **CSP no HTML consultado:** não encontrada pela busca realizada

## 5. Análise defensiva dos cabeçalhos

### X-Content-Type-Options

O valor `nosniff` orienta o navegador a não inferir determinados tipos de conteúdo por inspeção, ajudando a reduzir riscos associados à interpretação incorreta de tipos MIME.

### X-Frame-Options

O valor `SAMEORIGIN` restringe o enquadramento da página por documentos de outras origens e ajuda a mitigar determinados ataques de clickjacking.

### Feature-Policy

O cabeçalho `Feature-Policy` é uma versão antiga da política de permissões do navegador, sucedida por `Permissions-Policy`.

### Content-Security-Policy (CSP)

Não foi encontrada uma política CSP nos cabeçalhos HTTP da página principal nem na busca realizada no HTML consultado.

Uma CSP bem planejada pode ajudar a reduzir certos riscos de injeção de conteúdo. Entretanto, sua ausência na resposta examinada não comprova, isoladamente, uma vulnerabilidade explorável.

### Access-Control-Allow-Origin

O valor `*` permite que navegadores autorizem determinadas leituras entre origens, conforme as regras de CORS. Esse valor, isoladamente, não comprova uma vulnerabilidade.

## 6. Verificação de outros cabeçalhos

### Referrer-Policy

- **Resultado:** não encontrado na resposta HTTP examinada.
- **Análise:** sem uma política explícita, o navegador pode aplicar sua política padrão de envio do cabeçalho `Referer`.
- **Recomendação:** avaliar a configuração explícita de uma política adequada à aplicação.

### Permissions-Policy

- **Resultado:** não encontrado na resposta HTTP examinada.
- **Análise:** não foi identificada uma política explícita desse tipo na resposta consultada.
- **Recomendação:** avaliar restrições para recursos do navegador, como câmera, microfone e geolocalização, conforme a necessidade da aplicação.

### Strict-Transport-Security (HSTS)

- **Resultado:** não encontrado na resposta consultada via HTTP.
- **Análise:** a consulta utilizou `http://192.168.115.5:3000`. O HSTS é uma política recebida por HTTPS; portanto, sua ausência nessa resposta HTTP não foi classificada como vulnerabilidade.
- **Recomendação:** se a aplicação for disponibilizada por HTTPS em um ambiente apropriado, avaliar a configuração de HSTS conforme a arquitetura e os requisitos de segurança.

## 7. Verificação do método HTTP TRACE

- **Método testado:** `TRACE`
- **Status HTTP observado:** `200 OK`
- **Comportamento observado:** o servidor retornou HTML da página principal do OWASP Juice Shop.
- **Verificação de reflexão:** o marcador `X-Lab-Test: trace-check` não foi encontrado no corpo da resposta examinada.

**Análise:** os resultados não confirmam uma vulnerabilidade de reflexão via TRACE e também não permitem afirmar que o método esteja desabilitado. O status `200 OK`, isoladamente, não comprova o comportamento esperado do método.

## 8. Conclusão

A análise identificou cabeçalhos de segurança presentes e oportunidades de investigação relacionadas à CSP, à política de referência e à política de permissões.

Os resultados se limitam à página principal e às respostas examinadas. Outras rotas e respostas podem apresentar configurações diferentes.

Não foi confirmada uma vulnerabilidade explorável com base nas evidências registradas nesta atividade.

## 9. Escopo e uso responsável

A análise foi realizada exclusivamente no laboratório local autorizado. Não foram realizadas alterações na aplicação durante esta atividade.
