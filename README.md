# CyberLab — Laboratório de Cibersegurança

## Sobre o projeto

Este projeto documenta a construção de um laboratório local de cibersegurança para praticar reconhecimento de serviços, análise de tráfego HTTP e avaliação defensiva de configurações de segurança em aplicações web.

O objetivo é desenvolver habilidades práticas, registrar evidências técnicas e interpretar resultados com cautela, distinguindo configurações observadas de vulnerabilidades efetivamente confirmadas.

## Ambiente de laboratório

- **Virtualização:** Oracle VirtualBox
- **Sistema de análise:** Kali Linux
- **Sistema-alvo:** Ubuntu Linux
- **Aplicação web:** OWASP Juice Shop
- **Ferramentas:** Nmap, curl e Firefox
- **Rede:** ambiente local isolado
- **Aplicação de teste:** OWASP Juice Shop executado localmente

## Atividades realizadas

### 1. Reconhecimento de serviços com Nmap

Foi realizado um reconhecimento do sistema-alvo para identificar serviços de rede disponíveis.

- **Porta observada:** `22/tcp`
- **Serviço:** SSH
- **Resultado:** porta aberta
- **Evidência:** [`02-reconhecimento/nmap-ssh.txt`](02-reconhecimento/nmap-ssh.txt)

### 2. Análise de CORS

Foram examinados cabeçalhos HTTP de uma resposta da API e uma resposta ao método `OPTIONS`.

- Foi observado `Access-Control-Allow-Origin: *`.
- A resposta `OPTIONS` anunciou os métodos `GET`, `HEAD`, `PUT`, `PATCH`, `POST` e `DELETE`.
- As respostas examinadas não foram suficientes para confirmar uma vulnerabilidade de CORS.

**Conclusão:** a presença de um wildcard ou a divulgação de métodos HTTP, isoladamente, não comprova uma vulnerabilidade. A análise deve considerar os dados acessíveis, as credenciais, as políticas de origem e os controles de autorização.

**Evidência:** [`03-analise-cors.md`](03-analise-cors.md)

### 3. Análise de cabeçalhos HTTP de segurança

Foram inspecionados os cabeçalhos da página principal da aplicação.

Entre os resultados observados:

- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: SAMEORIGIN`
- `Feature-Policy: payment 'self'`
- `Access-Control-Allow-Origin: *`

Nas verificações realizadas, não foram encontrados `Content-Security-Policy`, `Referrer-Policy` e `Permissions-Policy` na resposta principal consultada.

O cabeçalho HSTS não foi encontrado na resposta HTTP examinada. Como o teste utilizou HTTP, esse resultado não permite concluir, por si só, que há uma falha de HSTS em uma configuração HTTPS.

**Conclusão:** os resultados apontam itens que podem ser investigados e endurecidos conforme a arquitetura e os requisitos da aplicação. A ausência de um cabeçalho em uma resposta específica não comprova, isoladamente, uma vulnerabilidade explorável.

**Evidência:** [`04-analise-cabecalhos.md`](04-analise-cabecalhos.md)

### 4. Verificação do método HTTP TRACE

Foi enviada uma solicitação utilizando o método `TRACE`.

- **Status HTTP observado:** `200 OK`
- O corpo retornado continha HTML da página principal.
- O marcador de teste `X-Lab-Test: trace-check` não foi encontrado no corpo examinado.

**Conclusão:** o teste não confirmou reflexão do cabeçalho por meio de TRACE. O status `200 OK`, isoladamente, também não comprova que o método foi executado com o comportamento esperado.

## Competências praticadas

- Reconhecimento básico de serviços com Nmap.
- Inspeção de respostas e cabeçalhos HTTP com curl.
- Análise inicial de CORS e métodos HTTP.
- Identificação de cabeçalhos de segurança.
- Registro de evidências técnicas em Markdown.
- Uso do Git para versionamento e do GitHub para publicação do projeto.
- Elaboração de conclusões baseadas nas evidências observadas.

## Estrutura do repositório

- `02-reconhecimento/` — evidência do reconhecimento de serviços.
- `03-analise-cors.md` — relatório de análise de CORS.
- `04-analise-cabecalhos.md` — relatório de cabeçalhos HTTP e método TRACE.
- `06-analise-trace.md` — análise do método TRACE e teste complementar de rota inexistente.
- `05-evidencias/` — arquivos de evidência dos testes realizados.
- `README.md` — apresentação e documentação geral do projeto.
- `.gitignore` — regras para excluir arquivos locais e temporários do versionamento.

## Conclusão

O projeto proporcionou prática em reconhecimento de rede e análise defensiva de aplicações web em um ambiente controlado. Os achados foram documentados com limites explícitos, sem classificar como vulnerabilidade algo que não foi confirmado pelas evidências disponíveis.

## Escopo e uso responsável

Todas as atividades foram realizadas no laboratório local para fins educacionais. Testes em sistemas de terceiros devem ser executados somente com autorização explícita.

## Ferramentas utilizadas

Kali Linux · Ubuntu Linux · Oracle VirtualBox · Nmap · curl · Firefox · OWASP Juice Shop · Git · GitHub
