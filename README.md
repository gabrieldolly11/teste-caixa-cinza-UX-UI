# Introdução

Projeto individual da disciplina UX/UI e Testes de Software, FACENS, professor Daniel Ohata. Responsável (atividade individual): **Gabriel Henrique Fernandes Torres — RA 251157**.

Este trabalho documenta testes funcionais de caixa cinza da autenticação por e-mail e senha do Supabase. O conhecimento parcial inclui endpoint, headers, formato JSON e comportamento esperado, sem exigir acesso ao código interno do serviço.

O enunciado define os requisitos. A Aula 11 orienta o planejamento dos cenários, a preparação do ambiente, a execução de chamadas HTTP e o registro de evidências (especialmente páginas 10–14 e 19–31). As etapas de interface FoodExpress e Selenium da aula ficam fora do escopo desta atividade. A planilha foi adaptada aos campos exigidos pelo enunciado, sem testes de aceitação de interface.

**Situação:** os cinco cenários foram executados pessoalmente no Postman Desktop e aprovados, totalizando 24 asserções. CT01 foi executado em 05/11/2026 e CT02–CT05 em 06/11/2026. Ambiente, arquivos e evidências foram preparados e conferidos com assistência técnica autorizada. Versão final publicada com autorização do aluno em 06/11/2026 no [repositório público da atividade](https://github.com/gabrieldolly11/teste-caixa-cinza-UX-UI), com acesso sem credenciais confirmado por HTTP 200.

# Objetivo da Atividade

Configurar autenticação no Supabase e verificar, no Postman Desktop, o login válido e a rejeição de quatro entradas inválidas. Comparar resultados esperados com HTTP, JSON, mensagens e presença/ausência de access_token, documentando os resultados reais em planilha e neste README.

Ferramentas obrigatórias: Supabase, Postman Desktop e GitHub. Os arquivos JSON e Excel foram preparados com ferramentas auxiliares; isso não constitui execução dos testes da API.

# Configuração do Supabase

O aluno entrou na conta Supabase. Em seguida, o ambiente já existente foi inspecionado no navegador com assistência autorizada em 05/11/2026. Não foi necessário criar outro projeto nem alterar as configurações de autenticação.

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

O aluno confirmou em 06/11/2026 que criou o projeto pelo painel Supabase e cadastrou o usuário em **Authentication > Users > Add user**, com confirmação de e-mail. O cadastro e a configuração já existiam antes da inspeção assistida; a criação não foi repetida. O fluxo utilizado é Dashboard > organização > New project, seguido da configuração do provedor Email e do cadastro do usuário. O estado de confirmação foi conferido no painel antes do teste válido. O e-mail e o identificador do usuário foram ocultados nas evidências públicas. Nome completo e RA foram informados pelo aluno em 06/11/2026 e estão registrados na Introdução.

Para a requisição com os três headers do enunciado, a chave pública anon legada está disponível. Copiar seu valor apenas para o ambiente local do Postman. A publishable também é compatível com a coleção mediante remoção automática de Authorization. Não foi necessário gerar novas chaves, revelar service_role ou consultar a chave secret.

A existência do usuário confirmado é uma pré-condição verificada. Após autorização do aluno, uma verificação auxiliar direta do endpoint de login retornou HTTP 200 e presença de access_token em 05/11/2026, confirmando as credenciais. O valor do token e a senha não foram gravados no projeto. Essa verificação foi executada pela assistência técnica fora do Postman Desktop e não substitui CT01 executado pessoalmente pelo aluno, posteriormente aprovado e registrado na planilha.

A chave de API identifica a aplicação; o e-mail e a senha autenticam o usuário. O access_token é um token de sessão retornado após login, **não é a api_key**. A terminologia da página 26 da aula deve ser interpretada com essa distinção.

Evidências reais da inspeção assistida. As únicas alterações nas imagens foram ocultações opacas de identificadores e da chave pública. Não foram alterados estados, datas ou resultados.

![01 — Projeto existente na organização](prints/01-supabase-projeto.png)

![02 — Painel do projeto com estado Healthy e URL](prints/02-supabase-painel.png)

![03 — Email habilitado e confirmação de e-mail exigida](prints/03-supabase-email.png)

![04 — Usuário existente com confirmação preenchida e identificadores ocultos](prints/04-supabase-usuario.png)

![05 — Localização da chave pública anon, com valor oculto e service_role não revelada](prints/05-supabase-api-protegida.png)

![06 — Conta atual identificada como YOU e Owner, com e-mail oculto](prints/06-supabase-responsavel.png)

# Configuração do Postman

O Postman Desktop 12.35.4 foi instalado a partir do [site oficial](https://www.postman.com/downloads/), após autorização do aluno. A assinatura digital do instalador foi verificada como válida, emitida para Postman, Inc. O aluno concluiu o login e o onboarding foi finalizado com perfil Student / Educator e equipe de uma pessoa. Foi criado o workspace **Teste Caixa Cinza — Supabase**, do tipo Internal e com acesso **Only you and invited people**, sem convidados adicionados. O cadeado e o indicador de um participante estão registrados nas capturas.

Os dois JSON revisados de [postman/](postman/) foram importados pelo diálogo **Use Resources > Import**, colando seu conteúdo JSON sem credenciais reais. A coleção apresenta CT01–CT05 e o Environment **Supabase — Ambiente de Testes** foi selecionado no canto superior direito. A importação foi confirmada pela interface. Os valores reais foram preenchidos depois, somente na coluna de valores locais do aplicativo; o arquivo de exemplo permaneceu com placeholders.

| Variável | Finalidade | Preenchimento local |
| --- | --- | --- |
| base_url | URL HTTPS do projeto, sem /auth/v1 | Project URL |
| api_key | Identificação pública do cliente | Publishable ou anon legada |
| email | Identificação do usuário de teste | Usuário cadastrado |
| password | Credencial do usuário | Senha local |

O JSON de exemplo contém apenas placeholders e não executa sem preenchimento. Na configuração realizada, base_url recebeu a URL do projeto, api_key recebeu a chave pública anon legada e email/password receberam as credenciais do usuário confirmado. Os indicadores de valores locais foram conferidos; o compartilhamento de password foi removido antes de inserir a senha real. As três variáveis sensíveis estão mascaradas. Nenhum ambiente preenchido foi exportado. O workspace foi configurado com acesso restrito. Para esta atividade, configure os valores apenas localmente; não os publique nem encaminhe o Environment real. Substituir pelos valores **locais**, sem usar Share/compartilhamento de valores. Marcar senha e chave como sensíveis. Dependendo da versão, a interface apresenta Value local ou campos Current/Initial; manter os valores compartilhados vazios/placeholder e nunca exportar o ambiente real para o repositório. Mascarar visualmente uma variável não equivale a impedir sincronização ou exportação.

Variáveis permitem usar o mesmo projeto em cinco requisições sem repetir credenciais nos arquivos. O pré-script exige as quatro variáveis, valida o domínio e bloqueia explicitamente chaves secretas/administrativas. Os scripts não armazenam tokens nem imprimem segredos.

Evidências reais da configuração assistida:

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

Com chave legada anon: `apikey: {{api_key}}`, `Authorization: Bearer {{api_key}}`, `Content-Type: application/json`. Com publishable: o pré-script **remove Authorization** e mantém apikey e Content-Type. Para cumprir literalmente os três headers do enunciado durante o teste, prefira usar a **chave pública anon legada** (se disponível no painel Supabase), pois ela é um JWT válido como `Bearer`. Se a plataforma disponibilizar apenas publishable, não invente um Bearer; registre no README a adaptação técnica e evidencie o header `Authorization` no modelo de requisição, explicando por que fica desabilitado no envio real. Publishable não é JWT e não deve ser enviada como Bearer. Nenhum access_token anterior é necessário para o login por senha. Essa adaptação preserva a intenção dos headers solicitados no enunciado com a API atual. Referência: [API keys](https://supabase.com/docs/guides/getting-started/api-keys).

Exemplo de body exigido pelo enunciado, **meramente demonstrativo**:

```json
{"email":"usuario@email.com","password":"123456"}
```

Modelo conceitual com variáveis:

```json
{"email":"{{email}}","password":"{{password}}"}
```

A coleção usa `JSON.stringify` no pré-script e variáveis locais `auth_email_json` e `auth_password_json` sem aspas adicionais no body. Assim senhas contendo aspas ou barras geram JSON válido. Essas variáveis são temporárias, não novos dados obrigatórios do Environment. Cada caso pode ser enviado independentemente, mantendo as pré-condições do ambiente.

Na preparação, CT01 foi aberto e seus parâmetros, headers e corpo foram conferidos. Depois dessa conferência, o aluno executou o envio pessoalmente, como registrado na seção seguinte. Os marcadores auth_email_json/auth_password_json são definidos pelo pré-script ao executar a requisição; sua indicação como não resolvidos antes da execução não é, por si só, falha de configuração.

![10 — Método POST e endpoint configurados](prints/10-postman-endpoint.png)

![11 — apikey, Authorization e Content-Type configurados com variáveis](prints/11-postman-headers.png)

![12 — Corpo raw JSON preparado, sem credenciais literais](prints/12-postman-body.png)

# Execução dos Testes

**EXECUÇÃO CONCLUÍDA:** CT01–CT05 enviados pessoalmente pelo aluno, com respostas e asserções conferidas no Postman Desktop. Todos os cinco casos foram aprovados.

| Cenário | Entrada utilizada (sem credenciais sensíveis) | Resultado esperado | Resultado obtido | Status |
| --- | --- | --- | --- | --- |
| CT01 — Login válido | E-mail cadastrado e senha correta | HTTP 200, JSON e access_token não vazio | HTTP 200; JSON válido; token presente e não vazio; 4/4 asserções | Sucesso |
| CT02 — Senha incorreta | E-mail cadastrado e senha deliberadamente diferente | Erro de autenticação; sem access_token | HTTP 400; invalid_credentials; Invalid login credentials; sem access_token. | Sucesso |
| CT03 — Usuário inexistente | E-mail não cadastrado e senha fictícia | Acesso negado; sem access_token | HTTP 400; invalid_credentials; Invalid login credentials; sem access_token. | Sucesso |
| CT04 — Campos vazios | E-mail e senha como strings vazias | Erro de validação; sem access_token | HTTP 400; validation_failed; missing email or phone; sem access_token. | Sucesso |
| CT05 — Credenciais inválidas | E-mail sem formato válido e senha fictícia | Mensagem de erro; sem access_token | HTTP 400; invalid_credentials; Invalid login credentials; sem access_token. | Sucesso |

Pré-condições conferidas: projeto ativo, provedor Email habilitado, usuário CT01 existente e confirmado, chave pública do mesmo projeto e ambiente selecionado. CT02 usa a senha correta acrescida de um sufixo, garantindo que a entrada seja diferente. CT03 gera um endereço com formato válido e identificador aleatório (prefixo `ct03-` e domínio `gmail.com`), para testar **usuário inexistente** e não simplesmente domínio inválido. Verificar em Authentication > Users que o endereço gerado não consta do projeto; não criar essa conta. Não registrar dados pessoais. CT04 usa strings vazias e aceita códigos de validação correspondentes (`validation_failed` ou `email_address_not_provided`). CT05 usa `email-sem-arroba` e senha fictícia.

O aluno abriu cada requisição e clicou em **Send**, uma por vez. A assistência conferiu o status HTTP, o JSON e a seção **Test Results**, capturou as evidências reais e registrou os resultados na planilha. Não foi utilizado o Runner para produzir os resultados apresentados. As cópias para publicação ocultam os valores integrais dos tokens do CT01 e os identificadores pessoais desnecessários. Nos casos negativos, a resposta foi registrada com seu código e mensagem de erro.

Os scripts usam `pm.test()` e `pm.expect()`. CT01 exige HTTP 200, objeto JSON e access_token presente e não vazio. Os negativos verificam JSON, ausência do campo token, informação de erro, HTTP 400, 401 ou 422 e código de erro coerente: invalid_credentials para CT02/03, validation_failed/email_address_not_provided para CT04, e invalid_credentials/validation_failed/email_address_invalid/email_address_not_provided para CT05, conforme a coleção revisada. Há compatibilidade por mensagem quando o código não existe. As expectativas foram definidas antes dos envios. Os códigos observados estão na tabela de resultados: 200 para CT01 e 400 para CT02–CT05. Se houver comportamento documentado diferente, registrar e investigar antes de ajustar o script, preservando o histórico.

Não basta qualquer HTTP de erro: 401/403 por chave inválida, 429 por limite ou 5xx por serviço indisponível não comprovam o cenário de credencial inválida. Não serão aprovados por estes scripts. Consultar [códigos de erro Auth](https://supabase.com/docs/guides/auth/debugging/error-codes).

Uma requisição concluída significa que houve resposta HTTP. Autenticação aceita significa que credenciais válidas geraram sessão. Autenticação corretamente rejeitada significa que a entrada inválida foi recusada pela razão esperada. Teste aprovado significa que as asserções corresponderam ao esperado; portanto, a rejeição correta de login inválido é um teste aprovado.

Evidência real do CT01: HTTP 200 OK, tempo exibido de 1,41 s e quatro asserções aprovadas: resposta JSON válida e objeto, HTTP 200, campo access_token presente e access token não vazio. Envio pessoal confirmado pelo aluno em 05/11/2026. Os tokens foram ocultados integralmente na cópia para publicação.

![13 — CT01, resposta real com tokens ocultos](prints/13-ct01-login-valido.png)

![13 — CT01, quatro asserções aprovadas no Postman Desktop](prints/13-ct01-login-valido-testes.png)

<!-- evidencia-CT02 -->

CT02 executado pessoalmente em 2026-11-06. HTTP 400; invalid_credentials; Invalid login credentials; sem access_token. As cinco asserções passaram.

![CT02 — resposta real](prints/14-ct02-senha-incorreta.png)

![CT02 — asserções aprovadas](prints/14-ct02-senha-incorreta-testes.png)

<!-- evidencia-CT03 -->

CT03 executado pessoalmente em 2026-11-06. HTTP 400; invalid_credentials; Invalid login credentials; sem access_token. As cinco asserções passaram.

![CT03 — resposta real](prints/15-ct03-usuario-inexistente.png)

![CT03 — asserções aprovadas](prints/15-ct03-usuario-inexistente-testes.png)

<!-- evidencia-CT04 -->

CT04 executado pessoalmente em 2026-11-06. HTTP 400; validation_failed; missing email or phone; sem access_token. As cinco asserções passaram.

![CT04 — resposta real](prints/16-ct04-campos-vazios.png)

![CT04 — asserções aprovadas](prints/16-ct04-campos-vazios-testes.png)

<!-- evidencia-CT05 -->

CT05 executado pessoalmente em 2026-11-06. HTTP 400; invalid_credentials; Invalid login credentials; sem access_token. As cinco asserções passaram.

![CT05 — resposta real](prints/17-ct05-credenciais-invalidas.png)

![CT05 — asserções aprovadas](prints/17-ct05-credenciais-invalidas-testes.png)

As capturas de resposta e Test Results acima correspondem às execuções pessoais. Algumas imagens foram recortadas para concentrar a evidência no painel da requisição; nenhum resultado foi modificado. Sugestões automáticas do aplicativo não foram usadas como critério de aprovação.

# Registro dos Testes

A [planilha editável](planilha/Registro_Testes.xlsx) registra os cinco cenários com ID, cenário, entrada, resultado esperado, resultado obtido, status e observações, além de data, código HTTP e referência das evidências. Todos os cinco casos têm status **Sucesso**. As datas foram registradas sem horário, pois não foi coletado um horário exato para os envios. Nenhuma senha real ou token integra a planilha.

A aba Resumo conta os status dos cinco casos principais em F7:F11 da aba Registro de Testes. Tentativas adicionais registradas fora dessas linhas não entram automaticamente nos indicadores. Manter nas cinco linhas principais o estado atual de cada caso e documentar o histórico das reexecuções nas observações ou em linhas adicionais identificadas.

Preencher após cada execução pessoal, sem senhas e tokens. Usar **Sucesso** quando todas as verificações pertinentes passarem, **Falha** quando houver divergência e **Bloqueado** quando um problema de ambiente impedir a avaliação. Registrar em observações a causa e qualquer reexecução. Manter histórico de tentativas relevantes em linhas adicionais, preservando a relação com o ID do caso. Se a entrada for sensível, descrevê-la por categoria.

O registro liga requisito, ação, resultado observado e prova. Evita concluir que uma falha de configuração valida um cenário negativo. Não foram identificadas divergências entre os resultados observados e os esperados nos cinco casos. Isso se limita à cobertura executada e não afirma ausência de defeitos em toda a plataforma.

A imagem a seguir é uma renderização da planilha efetivamente preenchida com os resultados observados, produzida após a atualização do arquivo Excel. Não é uma captura de uma sessão do aplicativo Excel. Como exemplos, CT01 registra HTTP 200 e token não vazio; CT04 registra HTTP 400 e validation_failed.

![18 — Planilha preenchida com os cinco resultados reais](prints/18-planilha-preenchida.png)

# Resultados Obtidos

**Cinco casos executados e aprovados, com 24 asserções aprovadas: quatro no CT01 e cinco em cada caso negativo.**

CT01: HTTP 200; JSON válido; access_token presente e não vazio. Execução pessoal em 2026-11-05. Evidências reais na seção Execução dos Testes.

CT02: HTTP 400; invalid_credentials; Invalid login credentials; sem access_token. Execução pessoal em 2026-11-06. Evidências reais na seção Execução dos Testes.

CT03: HTTP 400; invalid_credentials; Invalid login credentials; sem access_token. Execução pessoal em 2026-11-06. Evidências reais na seção Execução dos Testes.

CT04: HTTP 400; validation_failed; missing email or phone; sem access_token. Execução pessoal em 2026-11-06. Evidências reais na seção Execução dos Testes.

CT05: HTTP 400; invalid_credentials; Invalid login credentials; sem access_token. Execução pessoal em 2026-11-06. Evidências reais na seção Execução dos Testes.

A rejeição correta de credenciais inválidas foi considerada um teste aprovado. Nenhum caso foi aprovado apenas por retornar um HTTP de erro: as asserções também verificaram JSON, ausência de access_token e erro coerente com o cenário. Não houve divergência funcional confirmada nos cinco casos executados.

Dificuldades observadas na preparação: foi necessário instalar o Postman Desktop e concluir o login/onboarding para utilizar Collections e Workspaces. A senha do usuário foi verificada antes do teste acadêmico, sem necessidade de redefinição. Nenhuma dessas verificações auxiliares foi contabilizada como execução pessoal. Ao retomar em 06/11/2026, o aplicativo solicitou novo login e carregou os placeholders do Environment; foi necessário restaurar os valores locais antes de CT02. Essa dificuldade de configuração não foi classificada como defeito da API.

A conferência de requisitos está registrada no [checklist](docs/CHECKLIST.md), e as capturas estão relacionadas em [Evidências](docs/EVIDENCIAS.md). O .gitignore reduz inclusão acidental de ambientes e arquivos privados, mas não identifica segredos dentro de prints, planilhas ou arquivos renomeados. Revisar todo conteúdo e também o histórico Git antes de publicar.

**Link da entrega:** [teste-caixa-cinza-UX-UI](https://github.com/gabrieldolly11/teste-caixa-cinza-UX-UI). Versão final publicada e acesso público verificado em 06/11/2026. Repositório e README responderam HTTP 200 em requisições sem credenciais. Os 34 arquivos publicados, incluindo as 23 imagens, foram comparados ao pacote local por seus identificadores de conteúdo Git.

# Conclusão

Os cinco testes foram executados pessoalmente pelo aluno no Postman Desktop. O login válido retornou HTTP 200 e access_token não vazio. Senha incorreta, usuário inexistente e credenciais inválidas retornaram HTTP 400 com invalid_credentials; os campos vazios retornaram HTTP 400 com validation_failed. Todas as 24 asserções foram aprovadas. Portanto, a autenticação se comportou conforme o esperado nos cinco cenários avaliados.

As principais dificuldades foram a instalação e o acesso inicial ao Postman, a recuperação da configuração local após novo login e a distinção entre a chave pública da aplicação e o token de sessão do usuário. Foi utilizada a chave anon legada para manter os três headers do enunciado. Os valores sensíveis permaneceram no ambiente local e foram ocultados nas evidências públicas.

Não foram identificadas falhas funcionais nos casos executados. O HTTP 400 dos cenários negativos representa a rejeição esperada, e não reprovação do teste. No CT05, o serviço apresentou erro genérico de credenciais para o e-mail sem formato válido; a mensagem satisfez o resultado esperado desse caso. Os testes não demonstram proteção contra todos os ataques nem cobrem toda a API.

Como melhorias de cobertura, trabalhos futuros poderiam avaliar e-mail não confirmado, token expirado, revogação de sessão e limite de tentativas. Também seria útil acrescentar um caso separado de chave de API inválida, para diferenciar falha de acesso do cliente de rejeição das credenciais do usuário. Essas sugestões são extensões de teste, não defeitos comprovados nesta atividade.

Os testes de caixa cinza permitiram relacionar o conhecimento parcial do serviço — endpoint, headers, corpo JSON e mecanismo de autenticação — aos resultados observados. O registro de entradas, saídas, asserções e evidências tornou a avaliação verificável e evitou confundir resposta HTTP de erro com falha de um teste negativo.

Referências técnicas: Aula 11 fornecida pelo professor; enunciado da atividade fornecido pelo aluno; [autenticação por senha](https://supabase.com/docs/guides/auth/passwords), [chaves de API](https://supabase.com/docs/guides/getting-started/api-keys) e [erros Auth](https://supabase.com/docs/guides/auth/debugging/error-codes). Consultadas em 05/11/2026.
