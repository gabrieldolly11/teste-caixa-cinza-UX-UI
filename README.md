# Introdução

Sou **Gabriel Henrique Fernandes Torres — RA 251157** e desenvolvi este projeto individual para a disciplina UX/UI e Testes de Software, da FACENS, ministrada pelo professor Daniel Ohata.

Neste trabalho, documentei testes funcionais de caixa cinza da autenticação por e-mail e senha do Supabase. O conhecimento parcial inclui endpoint, headers, formato JSON e comportamento esperado, sem exigir acesso ao código interno do serviço.

Usei o enunciado como referência principal dos requisitos e a Aula 11 para orientar o planejamento dos cenários, a preparação do ambiente, a execução das chamadas HTTP e o registro de evidências (especialmente páginas 10–14 e 19–31). As etapas de interface FoodExpress e Selenium da aula ficam fora do escopo desta atividade. Adaptei a planilha aos campos exigidos pelo enunciado, sem incluir testes de aceitação de interface.

**Situação:** executei pessoalmente os cinco cenários no Postman Desktop, com aprovação dos cinco casos e das 24 asserções. Realizei CT01 em 05/11/2026 e CT02–CT05 em 06/11/2026. Contei com apoio técnico na preparação e conferência do ambiente, dos arquivos e das evidências. Publiquei a versão final em 06/11/2026 no [meu repositório público da atividade](https://github.com/gabrieldolly11/teste-caixa-cinza-UX-UI), cujo acesso sem credenciais foi confirmado por HTTP 200.

# Objetivo da Atividade

Meu objetivo foi configurar a autenticação no Supabase e verificar, no Postman Desktop, o login válido e a rejeição de quatro entradas inválidas. Comparei os resultados esperados com os códigos HTTP, o JSON, as mensagens e a presença ou ausência de access_token. Registrei os resultados reais na planilha e neste README.

Utilizei as ferramentas obrigatórias Supabase, Postman Desktop e GitHub. Para preparar os arquivos JSON e Excel, também contei com ferramentas auxiliares. Essa preparação foi separada da execução pessoal dos testes da API.

# Configuração do Supabase

Entrei na minha conta Supabase e, em 05/11/2026, conferi o ambiente que já havia criado, com apoio técnico. Como o projeto e a autenticação estavam configurados, aproveitei o ambiente existente.

| Item | Situação observada |
| --- | --- |
| Organização | Gabriel - Atividade API, plano Free |
| Projeto existente | Gabriel - Testes Caixa Cinza |
| Disponibilidade | Healthy no painel |
| Região | East US (Ohio), us-east-2 |
| Autenticação | Email habilitado em Authentication > Sign In / Providers |
| Confirm email | Habilitado |
| Usuário | Um registro visível com provedor email e estado Confirmed preenchido |
| Data exibida de criação e confirmação do usuário | 03 Nov, 2026 11:51, conforme o painel; não corresponde à data desta conferência |
| Conta autenticada | Perfil gabrieldolly11; a tela Team identifica a conta atual como YOU e Owner |
| Project URL | `https://xpuvyessnpxqowjgbfyh.supabase.co` |
| Chaves públicas disponíveis | Publishable e anon legada, em Settings > API Keys |

Criei o projeto pelo painel Supabase e cadastrei meu usuário de teste em **Authentication > Users > Add user**, com confirmação de e-mail. Segui o fluxo Dashboard > organização > New project e depois configurei o provedor Email e o cadastro. Antes do teste válido, conferi no painel que o usuário estava confirmado. Nas cópias das evidências para publicação, mantive ocultos o e-mail e o identificador do usuário.

Para utilizar os três headers do enunciado, escolhi a chave pública anon legada e mantive seu valor somente no ambiente local do Postman. A coleção também é compatível com publishable por meio da remoção automática de Authorization. Não precisei gerar novas chaves nem utilizar service_role ou secret.

Antes do CT01, autorizei uma verificação técnica auxiliar das credenciais, que retornou HTTP 200 e access_token em 05/11/2026. A senha e o valor do token não foram gravados no projeto. Como essa verificação ocorreu fora do Postman Desktop, não a contabilizei como execução da atividade: executei pessoalmente o CT01 no aplicativo e registrei esse resultado na planilha.

A chave de API identifica a aplicação; o e-mail e a senha autenticam o usuário. O access_token é um token de sessão retornado após login, **não é a api_key**. A terminologia da página 26 da aula deve ser interpretada com essa distinção.

As evidências abaixo registram o ambiente que utilizei. Na preparação das cópias públicas, foram aplicadas apenas ocultações opacas de identificadores e da chave pública, preservando estados, datas e resultados.

![01 — Projeto existente na organização](prints/01-supabase-projeto.png)

![02 — Painel do projeto com estado Healthy e URL](prints/02-supabase-painel.png)

![03 — Email habilitado e confirmação de e-mail exigida](prints/03-supabase-email.png)

![04 — Usuário existente com confirmação preenchida e identificadores ocultos](prints/04-supabase-usuario.png)

![05 — Localização da chave pública anon, com valor oculto e service_role não revelada](prints/05-supabase-api-protegida.png)

![06 — Conta atual identificada como YOU e Owner, com e-mail oculto](prints/06-supabase-responsavel.png)

# Configuração do Postman

Utilizei o Postman Desktop 12.35.4, instalado com apoio técnico a partir do [site oficial](https://www.postman.com/downloads/), com assinatura digital válida da Postman, Inc. Entrei na minha conta e configurei o perfil Student / Educator para uma equipe de uma pessoa. Preparei o workspace **Teste Caixa Cinza — Supabase**, do tipo Internal e com acesso **Only you and invited people**, sem adicionar convidados. As capturas mostram o cadeado e um único participante.

Importei, com apoio técnico, os dois JSON revisados de [postman/](postman/) pelo diálogo **Use Resources > Import**. A coleção ficou organizada em CT01–CT05, e selecionei o Environment **Supabase — Ambiente de Testes** no canto superior direito. Os arquivos importados não continham credenciais reais: esses valores foram preenchidos depois, apenas localmente, mantendo o arquivo de exemplo com placeholders.

| Variável | Finalidade | Preenchimento local |
| --- | --- | --- |
| base_url | URL HTTPS do projeto, sem /auth/v1 | Project URL |
| api_key | Identificação pública do cliente | Publishable ou anon legada |
| email | Identificação do usuário de teste | Usuário cadastrado |
| password | Credencial do usuário | Senha local |

Preenchi base_url com a URL do meu projeto, api_key com a chave pública anon legada e email/password com as credenciais do usuário confirmado. Mantive os valores reais apenas localmente, com acesso restrito ao workspace. O compartilhamento de password foi removido antes do preenchimento da senha, e as três variáveis sensíveis ficaram mascaradas. Não exportei o ambiente preenchido. O JSON de exemplo contém apenas placeholders e precisa ser preenchido localmente para reproduzir os testes. Também considerei que mascarar uma variável não impede, por si só, sua sincronização ou exportação.

Variáveis permitem usar o mesmo projeto em cinco requisições sem repetir credenciais nos arquivos. O pré-script exige as quatro variáveis, valida o domínio e bloqueia explicitamente chaves secretas/administrativas. Os scripts não armazenam tokens nem imprimem segredos.

Registrei a configuração nas evidências abaixo:

![07 — Workspace restrito criado para a atividade](prints/07-postman-workspace.png)

![08 — Environment selecionado, valores locais e credenciais mascaradas](prints/08-postman-environment.png)

![09 — Coleção importada com os cinco cenários](prints/09-postman-colecao.png)

# Configuração das Requisições

Coleção importável: **Supabase — Testes Caixa Cinza de Autenticação**.

Método **POST**. Endpoint: `{{base_url}}/auth/v1/token?grant_type=password`. O parâmetro seleciona autenticação por senha. O corpo envia as credenciais do usuário para obter uma sessão.

| Header | Valor demonstrativo | Função |
| --- | --- | --- |
| apikey | XXXXX | Identificar o cliente/projeto |
| Authorization | Bearer XXXXX (somente anon legada neste login) | Compatibilidade com a chave JWT legada |
| Content-Type | application/json | Indicar formato JSON do corpo |

Utilizei a chave anon legada com `apikey: {{api_key}}`, `Authorization: Bearer {{api_key}}` e `Content-Type: application/json`, atendendo aos três headers do enunciado. Mantive a compatibilidade da coleção com publishable: nesse caso, o pré-script **remove Authorization** e preserva os outros dois headers. Publishable não é JWT e não deve ser enviada como Bearer. Não precisei de um access_token anterior para realizar o login por senha. Consultei a documentação de [API keys](https://supabase.com/docs/guides/getting-started/api-keys) para distinguir esses tipos de chave.

Exemplo de body exigido pelo enunciado, **meramente demonstrativo**:

```json
{"email":"usuario@email.com","password":"123456"}
```

Modelo conceitual com variáveis:

```json
{"email":"{{email}}","password":"{{password}}"}
```

A coleção usa `JSON.stringify` no pré-script e variáveis locais `auth_email_json` e `auth_password_json` sem aspas adicionais no body. Assim senhas contendo aspas ou barras geram JSON válido. Essas variáveis são temporárias, não novos dados obrigatórios do Environment. Cada caso pode ser enviado independentemente, mantendo as pré-condições do ambiente.

Na preparação, conferi os parâmetros, headers e corpo do CT01 com apoio técnico. Depois, executei pessoalmente o envio, como registro na seção seguinte. Os marcadores auth_email_json/auth_password_json são definidos pelo pré-script ao executar a requisição; sua indicação como não resolvidos antes da execução não é, por si só, falha de configuração.

![10 — Método POST e endpoint configurados](prints/10-postman-endpoint.png)

![11 — apikey, Authorization e Content-Type configurados com variáveis](prints/11-postman-headers.png)

![12 — Corpo raw JSON preparado, sem credenciais literais](prints/12-postman-body.png)

# Execução dos Testes

**EXECUÇÃO CONCLUÍDA:** enviei pessoalmente CT01–CT05 no Postman Desktop e acompanhei a conferência das respostas e asserções. Os cinco casos foram aprovados.

| Cenário | Entrada utilizada (sem credenciais sensíveis) | Resultado esperado | Resultado obtido | Status |
| --- | --- | --- | --- | --- |
| CT01 — Login válido | E-mail cadastrado e senha correta | HTTP 200, JSON e access_token não vazio | HTTP 200; JSON válido; token presente e não vazio; 4/4 asserções | Sucesso |
| CT02 — Senha incorreta | E-mail cadastrado e senha deliberadamente diferente | Erro de autenticação; sem access_token | HTTP 400; invalid_credentials; Invalid login credentials; sem access_token. | Sucesso |
| CT03 — Usuário inexistente | E-mail não cadastrado e senha fictícia | Acesso negado; sem access_token | HTTP 400; invalid_credentials; Invalid login credentials; sem access_token. | Sucesso |
| CT04 — Campos vazios | E-mail e senha como strings vazias | Erro de validação; sem access_token | HTTP 400; validation_failed; missing email or phone; sem access_token. | Sucesso |
| CT05 — Credenciais inválidas | E-mail sem formato válido e senha fictícia | Mensagem de erro; sem access_token | HTTP 400; invalid_credentials; Invalid login credentials; sem access_token. | Sucesso |

Pré-condições conferidas: projeto ativo, provedor Email habilitado, usuário CT01 existente e confirmado, chave pública do mesmo projeto e ambiente selecionado. CT02 usa a senha correta acrescida de um sufixo, garantindo que a entrada seja diferente. CT03 gera um endereço com formato válido e identificador aleatório (prefixo `ct03-` e domínio `gmail.com`), para testar **usuário inexistente** e não simplesmente domínio inválido. Esse endereço foi gerado para o teste e não foi cadastrado como usuário do projeto. CT04 usa strings vazias e aceita códigos de validação correspondentes (`validation_failed` ou `email_address_not_provided`). CT05 usa `email-sem-arroba` e senha fictícia.

Abri cada requisição e cliquei em **Send**, uma por vez. Acompanhei, com apoio técnico, a conferência do status HTTP, do JSON e da seção **Test Results**, além do registro das evidências e dos resultados na planilha. Não utilizei o Runner para produzir os resultados apresentados. Nas cópias públicas, mantive ocultos os valores integrais dos tokens do CT01 e os identificadores pessoais desnecessários. Registrei os códigos e as mensagens dos cenários negativos.

Os scripts usam `pm.test()` e `pm.expect()`. CT01 exige HTTP 200, objeto JSON e access_token presente e não vazio. Os negativos verificam JSON, ausência do campo token, informação de erro, HTTP 400, 401 ou 422 e código de erro coerente: invalid_credentials para CT02/03, validation_failed/email_address_not_provided para CT04, e invalid_credentials/validation_failed/email_address_invalid/email_address_not_provided para CT05, conforme a coleção revisada. Há compatibilidade por mensagem quando o código não existe. As expectativas foram definidas antes dos envios. Os códigos observados estão na tabela de resultados: 200 para CT01 e 400 para CT02–CT05. Adotei como critério investigar qualquer divergência antes de alterar as expectativas dos scripts.

Não basta qualquer HTTP de erro: 401/403 por chave inválida, 429 por limite ou 5xx por serviço indisponível não comprovam o cenário de credencial inválida. Não serão aprovados por estes scripts. Usei a documentação de [códigos de erro Auth](https://supabase.com/docs/guides/auth/debugging/error-codes).

Uma requisição concluída significa que houve resposta HTTP. Autenticação aceita significa que credenciais válidas geraram sessão. Autenticação corretamente rejeitada significa que a entrada inválida foi recusada pela razão esperada. Teste aprovado significa que as asserções corresponderam ao esperado; portanto, a rejeição correta de login inválido é um teste aprovado.

Evidência real do CT01: HTTP 200 OK, tempo exibido de 1,41 s e quatro asserções aprovadas: resposta JSON válida e objeto, HTTP 200, campo access_token presente e access token não vazio. Realizei esse envio em 05/11/2026. Os tokens foram ocultados integralmente na cópia para publicação.

![13 — CT01, resposta real com tokens ocultos](prints/13-ct01-login-valido.png)

![13 — CT01, quatro asserções aprovadas no Postman Desktop](prints/13-ct01-login-valido-testes.png)

<!-- evidencia-CT02 -->

Executei o CT02 em 06/11/2026. HTTP 400; invalid_credentials; Invalid login credentials; sem access_token. As cinco asserções passaram.

![CT02 — resposta real](prints/14-ct02-senha-incorreta.png)

![CT02 — asserções aprovadas](prints/14-ct02-senha-incorreta-testes.png)

<!-- evidencia-CT03 -->

Executei o CT03 em 06/11/2026. HTTP 400; invalid_credentials; Invalid login credentials; sem access_token. As cinco asserções passaram.

![CT03 — resposta real](prints/15-ct03-usuario-inexistente.png)

![CT03 — asserções aprovadas](prints/15-ct03-usuario-inexistente-testes.png)

<!-- evidencia-CT04 -->

Executei o CT04 em 06/11/2026. HTTP 400; validation_failed; missing email or phone; sem access_token. As cinco asserções passaram.

![CT04 — resposta real](prints/16-ct04-campos-vazios.png)

![CT04 — asserções aprovadas](prints/16-ct04-campos-vazios-testes.png)

<!-- evidencia-CT05 -->

Executei o CT05 em 06/11/2026. HTTP 400; invalid_credentials; Invalid login credentials; sem access_token. As cinco asserções passaram.

![CT05 — resposta real](prints/17-ct05-credenciais-invalidas.png)

![CT05 — asserções aprovadas](prints/17-ct05-credenciais-invalidas-testes.png)

As capturas de resposta e Test Results acima correspondem às execuções pessoais. Algumas imagens foram recortadas para concentrar a evidência no painel da requisição; nenhum resultado foi modificado. Sugestões automáticas do aplicativo não foram usadas como critério de aprovação.

# Registro dos Testes

A [planilha editável](planilha/Registro_Testes.xlsx) registra os cinco cenários com ID, cenário, entrada, resultado esperado, resultado obtido, status e observações, além de data, código HTTP e referência das evidências. Todos os cinco casos têm status **Sucesso**. As datas foram registradas sem horário, pois não foi coletado um horário exato para os envios. Nenhuma senha real ou token integra a planilha.

A aba Resumo conta os status dos cinco casos principais em F7:F11 da aba Registro de Testes. Tentativas adicionais registradas fora dessas linhas não entram automaticamente nos indicadores. Mantive nas cinco linhas principais o resultado registrado de cada cenário.

Registrei os resultados após cada envio, sem incluir senhas ou tokens. Utilizei **Sucesso** quando o resultado correspondeu ao esperado. Reservei **Falha** para divergências funcionais e **Bloqueado** para impedimentos de ambiente. Descrevi as entradas sensíveis por categoria e associei cada registro ao ID do teste e à sua evidência.

O registro liga requisito, ação, resultado observado e prova. Evita concluir que uma falha de configuração valida um cenário negativo. Não identifiquei divergências entre os resultados observados e os esperados nos cinco casos. Isso se limita à cobertura executada e não afirma ausência de defeitos em toda a plataforma.

A imagem a seguir é uma renderização da planilha efetivamente preenchida com os resultados observados, produzida após a atualização do arquivo Excel. Não é uma captura de uma sessão do aplicativo Excel. Como exemplos, CT01 registra HTTP 200 e token não vazio; CT04 registra HTTP 400 e validation_failed.

![18 — Planilha preenchida com os cinco resultados reais](prints/18-planilha-preenchida.png)

# Resultados Obtidos

**Cinco casos executados e aprovados, com 24 asserções aprovadas: quatro no CT01 e cinco em cada caso negativo.**

CT01: HTTP 200; JSON válido; access_token presente e não vazio. Executei esse cenário em 05/11/2026. Evidências reais na seção Execução dos Testes.

CT02: HTTP 400; invalid_credentials; Invalid login credentials; sem access_token. Executei esse cenário em 06/11/2026. Evidências reais na seção Execução dos Testes.

CT03: HTTP 400; invalid_credentials; Invalid login credentials; sem access_token. Executei esse cenário em 06/11/2026. Evidências reais na seção Execução dos Testes.

CT04: HTTP 400; validation_failed; missing email or phone; sem access_token. Executei esse cenário em 06/11/2026. Evidências reais na seção Execução dos Testes.

CT05: HTTP 400; invalid_credentials; Invalid login credentials; sem access_token. Executei esse cenário em 06/11/2026. Evidências reais na seção Execução dos Testes.

Considerei a rejeição correta de credenciais inválidas como aprovação do teste. Não aprovei um caso apenas por retornar um HTTP de erro: as asserções também verificaram JSON, ausência de access_token e erro coerente com o cenário. Não houve divergência funcional confirmada nos cinco casos executados.

Durante a preparação, precisei instalar o Postman Desktop e concluir o login e a configuração inicial para utilizar Collections e Workspaces. Antes dos testes, autorizei a verificação auxiliar das credenciais e não precisei redefinir a senha. Não contabilizei essa verificação como execução pessoal da atividade. Ao retomar em 06/11/2026, o aplicativo solicitou novo login e carregou os placeholders do Environment; restaurei os valores locais com apoio técnico antes de executar CT02. Tratei essa situação como dificuldade de configuração, sem classificá-la como defeito da API.

A conferência de requisitos está registrada no [checklist](docs/CHECKLIST.md), e as capturas estão relacionadas em [Evidências](docs/EVIDENCIAS.md). O .gitignore reduz inclusão acidental de ambientes e arquivos privados, mas não identifica segredos dentro de prints, planilhas ou arquivos renomeados. Por isso, considerei a conferência dos arquivos e das imagens parte da preparação da publicação.

**Link da entrega:** [teste-caixa-cinza-UX-UI](https://github.com/gabrieldolly11/teste-caixa-cinza-UX-UI). Versão final publicada e acesso público verificado em 06/11/2026. Repositório e README responderam HTTP 200 em requisições sem credenciais. Os 34 arquivos publicados, incluindo as 23 imagens, foram comparados ao pacote local por seus identificadores de conteúdo Git.

# Conclusão

Executei pessoalmente os cinco testes no Postman Desktop. O login válido retornou HTTP 200 e access_token não vazio. Senha incorreta, usuário inexistente e credenciais inválidas retornaram HTTP 400 com invalid_credentials; os campos vazios retornaram HTTP 400 com validation_failed. Todas as 24 asserções foram aprovadas. Portanto, a autenticação se comportou conforme o esperado nos cinco cenários avaliados.

Minhas principais dificuldades foram a instalação e o acesso inicial ao Postman, a recuperação da configuração local após novo login e a distinção entre a chave pública da aplicação e o token de sessão do usuário. Utilizei a chave anon legada para manter os três headers do enunciado e preservei os valores sensíveis no ambiente local, com ocultação nas evidências públicas.

Não identifiquei falhas funcionais nos casos que executei. O HTTP 400 dos cenários negativos representa a rejeição esperada, e não reprovação do teste. No CT05, o serviço apresentou erro genérico de credenciais para o e-mail sem formato válido; a mensagem satisfez o resultado esperado desse caso. Os testes não demonstram proteção contra todos os ataques nem cobrem toda a API.

Como melhorias de cobertura, proponho avaliar e-mail não confirmado, token expirado, revogação de sessão e limite de tentativas em trabalhos futuros. Também acrescentaria um caso separado de chave de API inválida, para diferenciar uma falha de acesso do cliente da rejeição das credenciais do usuário. Essas sugestões são extensões de teste, não defeitos comprovados nesta atividade.

Com os testes de caixa cinza, consegui relacionar meu conhecimento parcial do serviço — endpoint, headers, corpo JSON e mecanismo de autenticação — aos resultados observados. Ao registrar entradas, saídas, asserções e evidências, mantive a avaliação verificável e distingui uma resposta HTTP de erro esperada de uma falha do teste.

Referências técnicas: Aula 11 fornecida pelo professor; enunciado da atividade; [autenticação por senha](https://supabase.com/docs/guides/auth/passwords), [chaves de API](https://supabase.com/docs/guides/getting-started/api-keys) e [erros Auth](https://supabase.com/docs/guides/auth/debugging/error-codes). Consultadas em 05/11/2026.
