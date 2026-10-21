# CyberLab — Laboratório de Cibersegurança

Laboratório prático de cibersegurança voltado ao reconhecimento de serviços, à análise de aplicações web e à avaliação defensiva de configurações e respostas HTTP.

**Objetivo:** desenvolver habilidades práticas em segurança da informação, documentar evidências técnicas e produzir análises fundamentadas em testes realizados em ambiente controlado.

## Sobre o projeto

O CyberLab foi desenvolvido em um ambiente local virtualizado para praticar conceitos de reconhecimento de rede, análise de protocolos HTTP, CORS, cabeçalhos de segurança, tratamento de erros e validação de entradas.

Os testes foram realizados contra uma instância local do OWASP Juice Shop. Os resultados foram registrados em relatórios Markdown e arquivos de evidência, com atenção à distinção entre comportamentos observados, riscos potenciais e vulnerabilidades efetivamente comprovadas.

## Ambiente de laboratório

| Componente | Tecnologia |
|---|---|
| Virtualização | Oracle VirtualBox |
| Sistema de análise | Kali Linux |
| Sistema-alvo | Ubuntu Linux |
| Aplicação web | OWASP Juice Shop |
| Ferramentas de análise | Nmap, curl e Firefox |
| Versionamento | Git e GitHub |
| Documentação | Markdown |

O alvo utilizado nos testes locais foi `192.168.115.5`, com a aplicação web acessível pela porta `3000`.

## Atividades realizadas

### 1. Reconhecimento de serviços

Foi realizado reconhecimento do sistema-alvo utilizando Nmap para identificar serviços de rede disponíveis.

- Serviço observado: SSH
- Porta: `22/tcp`
- Resultado: porta aberta
- Evidência: [`02-reconhecimento/nmap-ssh.txt`](02-reconhecimento/nmap-ssh.txt)

### 2. Análise de CORS

Foram examinados cabeçalhos HTTP e respostas relacionados à política de compartilhamento de recursos entre origens (CORS).

- Observação de `Access-Control-Allow-Origin: *`.
- Análise da resposta a uma solicitação `OPTIONS`.
- Avaliação contextual dos métodos HTTP anunciados.
- Registro das limitações dos testes realizados.

**Conclusão:** a presença de um wildcard ou a divulgação de métodos HTTP não comprova, isoladamente, uma vulnerabilidade. A avaliação depende dos recursos acessíveis, dos dados retornados e dos controles de autenticação e autorização.

Relatório: [`03-analise-cors.md`](03-analise-cors.md)

### 3. Análise de cabeçalhos HTTP de segurança

Foram inspecionados cabeçalhos de proteção retornados pela aplicação, incluindo:

- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: SAMEORIGIN`
- `Access-Control-Allow-Origin: *`
- `Feature-Policy: payment 'self'`

Também foram verificadas ausências de determinados cabeçalhos nas respostas examinadas.

**Conclusão:** os resultados permitem identificar configurações que merecem revisão, mas a ausência de um cabeçalho em uma resposta específica não comprova, por si só, uma vulnerabilidade explorável.

Relatório: [`04-analise-cabecalhos.md`](04-analise-cabecalhos.md)

### 4. Verificação do método HTTP TRACE

Foram realizadas solicitações utilizando o método `TRACE`, incluindo testes na página principal e em um endpoint da API.

Em um dos testes, a resposta apresentou `500 Internal Server Error`, acompanhado de detalhes técnicos e caminhos internos da aplicação.

**Conclusão:** foi observada exposição de informações técnicas na resposta de erro. Os testes não confirmaram, isoladamente, exploração bem-sucedida por meio do método TRACE.

Relatórios:
- [`06-analise-trace.md`](06-analise-trace.md)
- [`08-analise-trace-api.md`](08-analise-trace-api.md)

### 5. Análise de pesquisa e validação de entradas na API

Foram realizadas consultas ao endpoint de pesquisa de produtos do OWASP Juice Shop.

Os testes incluíram:

- Pesquisa por termos correspondentes a produtos.
- Pesquisa sem correspondências.
- Pesquisa com valor numérico.
- Envio controlado de uma entrada contendo caracteres especiais e uma expressão semelhante a SQL.

**Resultado relevante:** uma entrada especial provocou `500 Internal Server Error`, com a mensagem `SQLITE_ERROR: incomplete input`.

**Interpretação:** o resultado indica uma falha de processamento e exposição de detalhes técnicos. O teste isolado não comprova SQL injection explorável.

**Recomendações:** utilizar consultas parametrizadas, validar entradas conforme o formato esperado e retornar mensagens genéricas de erro aos clientes.

Relatório: [`09-analise-validacao-entrada.md`](09-analise-validacao-entrada.md)

### 6. Análise de rota inexistente e tratamento de erros

Foi testada uma rota inexistente da aplicação e, separadamente, um caminho inexistente sob `/api/`.

Os comportamentos observados foram diferentes:

- A rota fora do prefixo `/api/` retornou `200 OK` com o HTML principal, comportamento compatível com o fallback de uma aplicação SPA.
- O caminho `/api/rota-inexistente-lab` retornou `500 Internal Server Error`, com informações sobre o Express, caminhos internos e stack trace.

**Conclusão:** o segundo teste demonstrou exposição de informações técnicas na resposta. Em aplicações de produção, recomenda-se tratamento centralizado de erros e respostas apropriadas para rotas inexistentes.

Evidências:
- [`07-evidencias/erro-rota-inexistente.txt`](07-evidencias/erro-rota-inexistente.txt)
- [`07-evidencias/cabecalhos-erro-rota.txt`](07-evidencias/cabecalhos-erro-rota.txt)

### 7. Matriz consolidada de achados

Os resultados foram reunidos em uma matriz com identificadores, evidências, riscos preliminares e recomendações defensivas.

O documento diferencia:
- Erros observados.
- Exposição de informações técnicas.
- Configurações que merecem investigação adicional.
- Controles defensivos presentes nas respostas.
- Limitações dos testes realizados.

Documento: [`10-matriz-achados.md`](10-matriz-achados.md)

## Estrutura do repositório

| Caminho | Conteúdo |
|---|---|
| `02-reconhecimento/` | Evidências do reconhecimento com Nmap |
| `03-analise-cors.md` | Análise de CORS |
| `04-analise-cabecalhos.md` | Análise de cabeçalhos HTTP |
| `05-evidencias/` | Evidências de testes anteriores |
| `06-analise-trace.md` | Análise do método TRACE |
| `07-evidencias/` | Evidências dos testes complementares da API |
| `08-analise-trace-api.md` | Análise de TRACE em endpoint da API |
| `09-analise-validacao-entrada.md` | Pesquisa, entradas especiais e erros da API |
| `10-matriz-achados.md` | Matriz consolidada de achados |
| `README.md` | Apresentação geral do projeto |

## Competências praticadas

- Reconhecimento básico de serviços de rede com Nmap.
- Inspeção de respostas HTTP e cabeçalhos com curl.
- Análise inicial de CORS e métodos HTTP.
- Verificação de cabeçalhos de segurança.
- Investigação de erros de API e validação de entradas.
- Identificação e registro de exposição de informações técnicas.
- Coleta e organização de evidências reproduzíveis.
- Documentação de testes e recomendações defensivas em Markdown.
- Versionamento de alterações com Git e publicação no GitHub.
- Interpretação crítica dos resultados, respeitando os limites das evidências.

## Conclusão

O CyberLab proporcionou prática em reconhecimento de serviços e análise defensiva de uma aplicação web em ambiente controlado.

Os testes identificaram comportamentos relevantes para investigação, incluindo erros internos com exposição de detalhes técnicos e respostas HTTP que merecem avaliação contextual. As evidências foram organizadas e os resultados documentados com recomendações defensivas.

O projeto demonstra um fluxo de trabalho que envolve planejamento do teste, execução, coleta de evidências, interpretação dos resultados, documentação e versionamento.

Os achados são específicos ao ambiente examinado e não devem ser generalizados para outras aplicações sem testes adicionais.

## Escopo e uso responsável

Todas as atividades foram realizadas em laboratório local para fins educacionais. Testes em sistemas de terceiros devem ocorrer somente com autorização explícita e dentro do escopo permitido.

## Ferramentas utilizadas

Kali Linux · Ubuntu Linux · Oracle VirtualBox · Nmap · curl · Firefox · OWASP Juice Shop · Git · GitHub
