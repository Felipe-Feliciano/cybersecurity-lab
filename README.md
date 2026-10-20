# Projeto 1 — Laboratório de Cibersegurança

## Objetivo

Construir um laboratório local para praticar reconhecimento de serviços, análise de respostas HTTP e avaliação defensiva de configurações de segurança em aplicações web.

## Ambiente de laboratório

- **Sistema de análise:** Kali Linux
- **Sistema-alvo:** Ubuntu Linux
- **Aplicação web:** OWASP Juice Shop
- **Virtualização:** Oracle VirtualBox
- **Ferramentas:** Nmap, curl e navegador Firefox
- **Rede do laboratório:** rede local isolada do ambiente externo
- **Endereço da aplicação:** `http://192.168.115.5:3000`

## Atividades realizadas

### 1. Reconhecimento de serviços

Foi utilizado o Nmap para verificar o serviço SSH do sistema-alvo.

- Porta identificada: `22/tcp`
- Serviço: SSH
- Resultado observado: porta aberta
- Evidência: `02-reconhecimento/nmap-ssh.txt`

### 2. Análise de CORS

Foi examinada uma resposta da API do OWASP Juice Shop.

- Cabeçalho observado: `Access-Control-Allow-Origin: *`
- Foi consultada uma resposta `OPTIONS`.
- A resposta anunciou os métodos `GET`, `HEAD`, `PUT`, `PATCH`, `POST` e `DELETE`.
- Não foi confirmada uma vulnerabilidade de CORS com base nessas observações.

**Evidência:** `03-analise-cors.md`

### 3. Análise de cabeçalhos HTTP

Foram examinados os cabeçalhos da página principal da aplicação.

Cabeçalhos observados:

- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: SAMEORIGIN`
- `Feature-Policy: payment 'self'`
- `Access-Control-Allow-Origin: *`

Nas verificações realizadas, não foram encontrados `Content-Security-Policy`, `Referrer-Policy` e `Permissions-Policy` na resposta principal consultada.

O HSTS não foi encontrado na resposta HTTP utilizada no laboratório. Esse resultado, isoladamente, não caracteriza uma vulnerabilidade.

**Evidência:** `04-analise-cabecalhos.md`

### 4. Verificação do método HTTP TRACE

Foi enviada uma solicitação utilizando o método `TRACE`.

- Resposta observada: `HTTP 200 OK`
- O servidor retornou o HTML da página principal.
- O marcador `X-Lab-Test: trace-check` não foi encontrado no corpo da resposta.
- O teste não confirmou uma vulnerabilidade de reflexão via TRACE.

## Principais aprendizados

- Reconhecimento básico de serviços com Nmap.
- Inspeção de respostas HTTP com curl.
- Identificação e interpretação de cabeçalhos de segurança.
- Análise inicial de CORS e métodos HTTP.
- Registro de evidências e elaboração de conclusões técnicas cautelosas.

## Conclusão

O laboratório permitiu praticar técnicas básicas de reconhecimento e análise defensiva de aplicações web. Os resultados documentados estão limitados às solicitações e respostas efetivamente observadas. Não foi declarada nenhuma vulnerabilidade sem evidências suficientes.

## Escopo e segurança

Os testes foram realizados no laboratório local criado para fins educacionais. As atividades descritas não autorizam testar sistemas de terceiros sem permissão.

## Ferramentas

- Kali Linux
- Ubuntu Linux
- Oracle VirtualBox
- Nmap
- curl
- OWASP Juice Shop
- Firefox
