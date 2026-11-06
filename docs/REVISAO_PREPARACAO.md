# Histórico da revisão de preparação

> Documento histórico: as verificações abaixo pertencem à preparação, anterior aos envios no Postman. Os estados “pendente”, “sem capturas” e os totais 0/0/0/5 descrevem exclusivamente aquela fase.

## Situação final em 06/11/2026

| Caso | Data da execução | HTTP observado | Asserções aprovadas | Status |
| --- | --- | --- | --- | --- |
| CT01 | 05/11/2026 | 200 | 4/4 | Sucesso |
| CT02 | 06/11/2026 | 400 | 5/5 | Sucesso |
| CT03 | 06/11/2026 | 400 | 5/5 | Sucesso |
| CT04 | 06/11/2026 | 400 | 5/5 | Sucesso |
| CT05 | 06/11/2026 | 400 | 5/5 | Sucesso |

Os cinco casos foram enviados individualmente no Postman Desktop. O registro final soma 24 asserções aprovadas; a aba Resumo contém cinco sucessos, zero falhas, zero bloqueados e zero pendentes. As capturas 01–18 (23 imagens) e a planilha preenchida estão na entrega. A publicação inicial foi concluída em 06/11/2026.

No CT03, as capturas comprovam HTTP 400 com `invalid_credentials`, ausência de `access_token` e cinco asserções aprovadas. O endereço é gerado aleatoriamente com formato válido pelo pré-script. Não há, no conjunto disponível, evidência de consulta que comprove diretamente a ausência desse endereço em Authentication > Users.

O estado final está no [README](../README.md), na [planilha](../planilha/Registro_Testes.xlsx) e nas [evidências](EVIDENCIAS.md). O roteiro abaixo permanece como registro das verificações e providências anteriores, sem pendências de execução atuais.

## Registro anterior à execução

Revisão técnica inicial da preparação em 05/11/2026. As seções abaixo registram a situação antes da inspeção do Supabase. Posteriormente, o aluno entrou na conta e autorizou a conferência assistida: projeto existente Healthy, Email habilitado, Confirm email habilitado, usuário confirmado e chaves públicas disponíveis foram verificados. As seis capturas reais estão em prints/ e incorporadas no README. Nenhum login por senha foi executado nesta conferência. Naquele momento, a planilha ainda continha os cinco casos pendentes; esse estado foi substituído pelos registros de execução de 05 e 06/11/2026.

## Base preservada

- Utilizado o ZIP revisado encontrado em Downloads. As sete cópias têm conteúdo binário idêntico.
- README e planilha anexados separadamente são idênticos aos arquivos correspondentes do ZIP. Não foram criadas cópias adicionais dentro do projeto.
- Planilha preservada sem reexportação: abas, fórmulas, validação de status e formatação mantidas.
- Coleção e Environment revisados preservados sem alterações.

## Verificações realizadas antes da execução

| Item | Conferência | Situação da preparação |
| --- | --- | --- |
| Estrutura | README.md, postman/, prints/, planilha/ e documentos auxiliares | Correta |
| README | Exatamente nove seções principais na ordem do enunciado | Correta |
| Coleção | CT01–CT05, POST e endpoint de login por senha | Presente |
| Environment | base_url, api_key, email e password com placeholders | Presente |
| Pós-resposta | 25 verificações locais com respostas sintéticas, inclusive chave inválida, 429 e 503 | Lógica verificada localmente |
| Pré-requisição | 20 combinações locais de cenário/tipo de chave, serialização JSON e bloqueio de chaves administrativas | Lógica verificada localmente |
| Planilha | Quatro COUNTIF recalculadas no motor auxiliar: 0 sucessos, 0 falhas, 0 bloqueados e 5 pendentes | Correta |
| Dependências do resumo | Alteração temporária em memória para Sucesso/Falha/Bloqueado produziu 1/1/1 e 2 pendentes; valores restaurados em memória, sem salvar no XLSX | Conferida |
| Visual | Duas abas renderizadas e inspecionadas | Legíveis |
| Credenciais | Environment contém somente placeholders; não há capturas reais no pacote | Sem credenciais reais identificadas nos arquivos inspecionados |

Os testes locais exercitam a lógica dos scripts com objetos auxiliares. Não são execução no Postman Desktop nem evidências acadêmicas do Supabase. Naquela fase, a importação e o funcionamento no Postman Desktop ainda precisavam de confirmação; ambos foram confirmados na etapa prática posterior. O resumo não incorpora automaticamente tentativas fora de F7:F11.

## Ajustes pontuais

1. README alinhado ao código adicional email_address_not_provided já aceito pelo CT05 na coleção revisada. Não houve alteração de asserções ou registro de resultados.
2. README esclarece o alcance das fórmulas da aba Resumo e o registro de reexecuções.
3. Guia detalha a criação do projeto, a importação dos dois JSON e as primeiras capturas, com referências oficiais.

## Pendências da fase de preparação — posteriormente concluídas

Configuração pessoal do Supabase; configuração/importação no Postman Desktop; execução pessoal dos cinco casos; capturas 01–18; preenchimento dos resultados; identificação do aluno; conclusão baseada em evidências; revisão de privacidade das imagens; aprovação do aluno antes da publicação; GitHub público e acessível.

Próxima ação prevista naquele momento: abrir o painel Supabase e criar ou acessar o projeto dedicado, conforme docs/GUIA_EXECUCAO.md. Naquele momento, a confirmação dependia do retorno do responsável e da conferência das evidências reais; essas confirmações ocorreram na continuação da atividade.
