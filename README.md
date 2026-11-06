# Introdução

Projeto individual de testes funcionais de caixa cinza da autenticação por e-mail e senha do Supabase, com requisições no Postman Desktop e registro dos resultados em planilha.

**Gabriel Henrique Fernandes Torres — RA 251157**  
FACENS · UX/UI e Testes de Software · Professor Daniel Ohata

Este guia reúne a configuração do ambiente, os cinco cenários de teste e suas evidências. A metodologia segue o enunciado da atividade e a Aula 11: planejamento, preparação, execução e comparação dos resultados esperados com os observados. A abordagem de caixa cinza utiliza conhecimento do endpoint, dos headers, do corpo JSON e do funcionamento da autenticação.

**Resultado:** cinco casos aprovados e 24 asserções aprovadas. CT01 foi executado em 05/11/2026; CT02–CT05, em 06/11/2026.

# Objetivo da Atividade

Verificar o comportamento do login por senha em um cenário válido e quatro cenários inválidos, considerando o status HTTP, o conteúdo JSON, as mensagens de erro e a presença ou ausência de `access_token`.

Os arquivos necessários para reprodução estão em [postman/](postman/), os registros em [planilha/Registro_Testes.xlsx](planilha/Registro_Testes.xlsx) e as capturas em [prints/](prints/). O escopo é a autenticação pela API; testes de interface FoodExpress e Selenium apresentados na aula não fazem parte desta atividade.

# Configuração do Supabase

O ambiente utiliza a seguinte configuração:

| Item | Configuração |
| --- | --- |
| Organização | Gabriel - Atividade API · Free |
| Projeto | Gabriel - Testes Caixa Cinza |
| Estado observado | Healthy |
| Região | East US (Ohio), us-east-2 |
| Project URL | `https://xpuvyessnpxqowjgbfyh.supabase.co` |
| Provedor Email | Habilitado |
| Confirm email | Habilitado |
| Usuário de teste | Cadastrado com provedor email e confirmação preenchida |

Para configurar um ambiente equivalente:

1. No Dashboard, selecionar a organização e criar o projeto em **New project**.
2. Em **Authentication > Sign In / Providers**, habilitar Email e a confirmação de e-mail.
3. Em **Authentication > Users > Add user**, cadastrar o usuário de teste com e-mail confirmado.
4. Em **Settings > API Keys**, localizar a chave pública e copiar a Project URL para o ambiente do Postman.

A execução documentada utiliza a chave pública **anon legada**, compatível com os três headers do enunciado. A coleção também aceita publishable, removendo automaticamente o header Authorization nesse caso. Chaves administrativas `service_role` e secret não são utilizadas.

A chave de API identifica a aplicação. Já o e-mail e a senha autenticam o usuário, e o `access_token` retornado representa sua sessão. Esses valores têm funções diferentes.

As capturas mostram o projeto, o provedor Email, o usuário confirmado e a localização da chave pública. Identificadores pessoais e valores de chave foram ocultados nas cópias públicas.

![01 — Projeto existente na organização](prints/01-supabase-projeto.png)

![02 — Painel do projeto com estado Healthy e URL](prints/02-supabase-painel.png)

![03 — Email habilitado e confirmação de e-mail exigida](prints/03-supabase-email.png)

![04 — Usuário existente com confirmação preenchida e identificadores ocultos](prints/04-supabase-usuario.png)

![05 — Localização da chave pública anon, com valor oculto e service_role não revelada](prints/05-supabase-api-protegida.png)

![06 — Conta atual identificada como YOU e Owner, com e-mail oculto](prints/06-supabase-responsavel.png)

# Configuração do Postman

Ambiente utilizado: **Postman Desktop 12.35.4**, disponível no [site oficial](https://www.postman.com/downloads/).

1. Criar o workspace **Teste Caixa Cinza — Supabase**, com acesso restrito em **Only you and invited people**.
2. Importar os arquivos da pasta [postman/](postman/) em **Use Resources > Import**.
3. Selecionar o Environment **Supabase — Ambiente de Testes**.
4. Preencher as quatro variáveis com os dados do projeto e do usuário confirmado.

| Variável | Valor necessário |
| --- | --- |
| `base_url` | Project URL, sem `/auth/v1` |
| `api_key` | Chave pública anon legada ou publishable |
| `email` | E-mail do usuário cadastrado |
| `password` | Senha do usuário |

O arquivo de exemplo contém apenas placeholders. Os valores reais ficam no ambiente local; o ambiente preenchido não integra a entrega. Na configuração registrada, o compartilhamento de `password` foi removido antes do preenchimento, e os valores de chave, e-mail e senha ficaram mascarados. O mascaramento visual, isoladamente, não impede sincronização ou exportação.

A coleção **Supabase — Testes Caixa Cinza de Autenticação** contém cinco requisições independentes. O pré-script valida as variáveis, confere o domínio e bloqueia chaves administrativas. Os scripts não registram segredos no console nem armazenam tokens.

![07 — Workspace restrito criado para a atividade](prints/07-postman-workspace.png)

![08 — Environment selecionado, valores locais e credenciais mascaradas](prints/08-postman-environment.png)

![09 — Coleção importada com os cinco cenários](prints/09-postman-colecao.png)

# Configuração das Requisições

**Método:** `POST`  
**Endpoint:** `{{base_url}}/auth/v1/token?grant_type=password`

O parâmetro `grant_type=password` seleciona o login por senha. Para a chave anon legada, os headers são:

| Header | Valor |
| --- | --- |
| `apikey` | `{{api_key}}` |
| `Authorization` | `Bearer {{api_key}}` |
| `Content-Type` | `application/json` |

Quando a variável contém uma chave publishable, o pré-script remove `Authorization`, pois esse tipo de chave não é JWT. O login por senha não exige um token de sessão anterior.

Modelo conceitual do body, em **raw / JSON**:

```json
{"email":"{{email}}","password":"{{password}}"}
```

Na coleção, o pré-script usa `JSON.stringify` e as variáveis temporárias `auth_email_json` e `auth_password_json`, sem aspas adicionais no body. Isso preserva o JSON válido mesmo quando a senha contém aspas ou barras. Essas variáveis são definidas durante a execução e não precisam ser adicionadas ao Environment.

![10 — Método POST e endpoint configurados](prints/10-postman-endpoint.png)

![11 — apikey, Authorization e Content-Type configurados com variáveis](prints/11-postman-headers.png)

![12 — Corpo raw JSON preparado, sem credenciais literais](prints/12-postman-body.png)

# Execução dos Testes

Antes de enviar as requisições, conferir se o projeto está ativo, o provedor Email está habilitado, o usuário válido está confirmado e o Environment correto está selecionado.

Abrir cada caso, clicar em **Send** e conferir o status HTTP, o JSON e a aba **Test Results**. Os resultados apresentados foram obtidos por envios individuais no Postman Desktop.

| Caso | Entrada | Resultado esperado | Resultado observado | Status |
| --- | --- | --- | --- | --- |
| CT01 — Login válido | E-mail cadastrado e senha correta | HTTP 200 e token não vazio | HTTP 200; JSON válido; `access_token` presente e não vazio; 4/4 asserções | Sucesso |
| CT02 — Senha incorreta | E-mail cadastrado e senha diferente | Rejeição da autenticação, sem token | HTTP 400; `invalid_credentials`; 5/5 asserções | Sucesso |
| CT03 — Usuário inexistente | E-mail não cadastrado e senha fictícia | Acesso negado, sem token | HTTP 400; `invalid_credentials`; 5/5 asserções | Sucesso |
| CT04 — Campos vazios | E-mail e senha vazios | Erro de validação, sem token | HTTP 400; `validation_failed`; 5/5 asserções | Sucesso |
| CT05 — Credenciais inválidas | E-mail sem formato válido e senha fictícia | Mensagem de erro, sem token | HTTP 400; `invalid_credentials`; 5/5 asserções | Sucesso |

CT02 acrescenta um sufixo à senha correta, garantindo uma entrada diferente. CT03 gera um endereço de formato válido com prefixo `ct03-`, identificador aleatório e domínio `gmail.com`; o endereço não é cadastrado no projeto. CT04 envia strings vazias. CT05 utiliza `email-sem-arroba`.

As verificações usam `pm.test()` e `pm.expect()`. CT01 exige HTTP 200, objeto JSON e token presente e não vazio. Nos cenários negativos, as asserções conferem JSON, ausência de `access_token`, informação de erro e correspondência com o cenário. A coleção aceita HTTP 400, 401 ou 422 quando acompanhado do erro esperado. Erros de chave de API, limite de requisições e indisponibilidade do serviço não validam os cenários de credenciais inválidas.

CT02, CT03 e CT05 retornaram a mensagem `Invalid login credentials`. CT04 retornou `missing email or phone`. A rejeição correta é o resultado esperado nesses casos e, portanto, representa aprovação do teste.

As evidências abaixo registram as respostas e as asserções de cada caso. Os tokens do CT01 foram ocultados integralmente. Alguns recortes concentram a imagem no painel da requisição, preservando os resultados.

![13 — CT01, resposta real com tokens ocultos](prints/13-ct01-login-valido.png)

![13 — CT01, quatro asserções aprovadas no Postman Desktop](prints/13-ct01-login-valido-testes.png)

![CT02 — resposta real](prints/14-ct02-senha-incorreta.png)

![CT02 — asserções aprovadas](prints/14-ct02-senha-incorreta-testes.png)

![CT03 — resposta real](prints/15-ct03-usuario-inexistente.png)

![CT03 — asserções aprovadas](prints/15-ct03-usuario-inexistente-testes.png)

![CT04 — resposta real](prints/16-ct04-campos-vazios.png)

![CT04 — asserções aprovadas](prints/16-ct04-campos-vazios-testes.png)

![CT05 — resposta real](prints/17-ct05-credenciais-invalidas.png)

![CT05 — asserções aprovadas](prints/17-ct05-credenciais-invalidas-testes.png)

# Registro dos Testes

A [planilha Registro_Testes.xlsx](planilha/Registro_Testes.xlsx) contém as abas **Registro de Testes** e **Resumo**. Cada caso registra ID, cenário, entrada, resultado esperado, resultado obtido, status, observações, data, código HTTP e referência da evidência.

Os cinco registros estão classificados como **Sucesso**. Esse status indica correspondência com o resultado esperado; **Falha** indica divergência funcional e **Bloqueado** indica impedimento de execução. As entradas sensíveis são descritas por categoria, sem senhas ou tokens.

A aba Resumo contabiliza os status das cinco linhas principais, em `F7:F11` da aba Registro de Testes. As datas correspondem aos dias de execução, sem horários não registrados.

A imagem abaixo é uma renderização do arquivo Excel preenchido, utilizada para facilitar a consulta.

![18 — Planilha preenchida com os cinco resultados reais](prints/18-planilha-preenchida.png)

# Resultados Obtidos

| Indicador | Total |
| --- | --- |
| Casos executados | 5 |
| Casos aprovados | 5 |
| Casos com falha | 0 |
| Casos bloqueados ou pendentes | 0 |
| Asserções aprovadas | 24 |

O login válido retornou HTTP 200 e token de sessão. Os quatro cenários negativos retornaram HTTP 400, sem `access_token`, com erros coerentes com as entradas. Não houve divergência funcional nos cinco casos avaliados.

A preparação exigiu a instalação e o acesso inicial ao Postman. Na retomada, um novo login carregou os placeholders do Environment, exigindo o preenchimento dos valores locais antes dos testes restantes. Essa ocorrência foi tratada como ajuste do ambiente.

A conferência dos requisitos está em [CHECKLIST.md](docs/CHECKLIST.md), e a relação das capturas está em [EVIDENCIAS.md](docs/EVIDENCIAS.md).

**Entrega:** [repositório público teste-caixa-cinza-UX-UI](https://github.com/gabrieldolly11/teste-caixa-cinza-UX-UI). O pacote reúne 34 arquivos, incluindo 23 imagens. A publicação e o acesso público foram verificados em 06/11/2026.

# Conclusão

A autenticação apresentou o comportamento esperado nos cinco cenários: criação de sessão com credenciais válidas e rejeição das entradas inválidas. As 24 asserções aprovadas e as evidências permitem conferir os resultados registrados.

O conhecimento do endpoint, dos headers e do formato das respostas orientou a construção e a interpretação dos testes de caixa cinza. A análise conjunta do status HTTP, do erro e do token evita confundir uma rejeição esperada com falha do teste.

Essa conclusão se limita aos cenários executados. Possíveis ampliações incluem e-mail não confirmado, token expirado, revogação de sessão, limite de tentativas e chave de API inválida.

Referências: Aula 11 e enunciado da atividade; documentação do Supabase sobre [autenticação por senha](https://supabase.com/docs/guides/auth/passwords), [chaves de API](https://supabase.com/docs/guides/getting-started/api-keys) e [erros de autenticação](https://supabase.com/docs/guides/auth/debugging/error-codes).
