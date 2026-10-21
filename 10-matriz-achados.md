# Matriz de Achados de Segurança — OWASP Juice Shop

## 1. Objetivo

Consolidar os resultados dos testes realizados no laboratório local OWASP Juice Shop, registrar os riscos potenciais e propor medidas defensivas.

## 2. Escopo do teste

- Aplicação: OWASP Juice Shop
- Ambiente: laboratório local em VirtualBox
- Cliente: Kali Linux
- Alvo: `192.168.115.5:3000`
- Ferramentas: `curl` e Nmap
- Finalidade: estudo educacional e análise defensiva

## 3. Matriz de achados

| ID | Achado observado | Evidência | Risco preliminar | Situação |
|---|---|---|---|---|
| A01 | Entrada especial na pesquisa provocou erro SQLite | `07-evidencias/erro-validacao-api.txt` | Médio — a confirmar | Erro e detalhe técnico observados; SQL injection explorável não comprovada |
| A02 | Rota inexistente retornou HTTP 500 com stack trace | `07-evidencias/erro-rota-inexistente.txt` | Baixo a médio — a confirmar | Exposição de informações técnicas observada |
| A03 | Resposta de erro contém `Access-Control-Allow-Origin: *` | `07-evidencias/cabecalhos-erro-rota.txt` | Indeterminado | Política observada; impacto não comprovado |
| A04 | Cabeçalhos `X-Content-Type-Options: nosniff` e `X-Frame-Options: SAMEORIGIN` presentes | `07-evidencias/cabecalhos-erro-rota.txt` | Controle defensivo observado | Presença confirmada; não representa garantia de segurança completa |
| A05 | Pesquisa por termos válidos e sem correspondência retornou HTTP 200 | `07-evidencias/analise-api-produtos.txt` e `07-evidencias/busca-juice.txt` | Sem risco específico demonstrado | Comportamento compatível com pesquisa normal |

## 4. Análise dos riscos

### A01 — Erro SQLite na pesquisa

Uma entrada contendo caracteres especiais provocou uma resposta HTTP 500 com mensagem relacionada ao SQLite.

**Impacto potencial:** exposição de detalhes internos e possível fragilidade no tratamento de entradas.

**Limitação:** esse teste isolado não comprova SQL injection explorável.

**Recomendação:** utilizar consultas parametrizadas, validar entradas e retornar mensagens genéricas de erro.

### A02 — Exposição de stack trace

A consulta a uma rota inexistente retornou caminhos internos, informações do Express e detalhes da execução.

**Impacto potencial:** facilitar o reconhecimento da estrutura interna da aplicação.

**Recomendação:** tratar erros de forma centralizada e não enviar stack traces ao cliente em produção.

### A03 — Política CORS permissiva

Foi observado `Access-Control-Allow-Origin: *` na resposta de erro e em respostas anteriores da aplicação.

**Impacto potencial:** depende dos recursos acessíveis, da autenticação e dos dados retornados.

**Limitação:** o cabeçalho, sozinho, não demonstra acesso indevido a informações sensíveis.

**Recomendação:** restringir as origens quando necessário e avaliar a política CORS conforme os requisitos da aplicação.

### A04 — Cabeçalhos de proteção

Os cabeçalhos `nosniff` e `SAMEORIGIN` estavam presentes na resposta examinada.

**Interpretação:** são controles defensivos úteis para riscos específicos do navegador, mas não corrigem erros internos nem substituem outras medidas de segurança.

### A05 — Comportamento da pesquisa

As pesquisas realizadas retornaram resultados para termos correspondentes e listas vazias para consultas sem correspondências.

**Interpretação:** os testes observados foram compatíveis com o funcionamento normal da pesquisa. Isso não exclui outras falhas.

## 5. Recomendações gerais

1. Adotar consultas parametrizadas no acesso ao banco de dados.
2. Padronizar o tratamento de erros HTTP.
3. Evitar a exposição de stack traces e caminhos internos.
4. Revisar as permissões CORS conforme a necessidade real da aplicação.
5. Preservar evidências reproduzíveis dos testes.
6. Executar novos testes somente no ambiente autorizado.
7. Revisar os resultados antes de atribuir uma classificação definitiva de risco.

## 6. Conclusão

Os testes identificaram exposição de informações técnicas em respostas de erro e um erro SQLite provocado por uma entrada especial. Também foram observados cabeçalhos de proteção e uma política CORS permissiva.

Os resultados documentam comportamentos relevantes para investigação defensiva, mas não comprovam, isoladamente, SQL injection explorável ou acesso indevido a dados sensíveis.

Os níveis de risco apresentados são preliminares e devem ser reavaliados conforme a exposição, a possibilidade de exploração e o impacto demonstrado.

## 7. Contexto

Todos os testes foram realizados no laboratório educacional local com OWASP Juice Shop, sem autorização ou intenção de testar sistemas de terceiros.
