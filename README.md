# Introdução

**Gabriel Henrique Fernandes Torres — RA 251157**  
Centro Universitário Facens · UX/UI e Testes de Software · Professor Dr. Daniel Ohata.

Esta atividade aborda testes de caixa cinza na API de autenticação do Supabase: são conhecidos endpoint, headers e formato das entradas e respostas, sem inspeção integral da implementação interna. A interface gráfica não faz parte do escopo.

**Estado: preparação concluída; configuração do ambiente, execução pessoal e evidências pendentes. Este documento ainda não constitui entrega final.**

# Objetivo da Atividade

Configurar autenticação por e-mail, executar pessoalmente cinco cenários no Postman Desktop e comparar entradas, status HTTP e respostas JSON. Documentar cada resultado com evidência produzida durante a execução e registro correspondente na planilha.

# Configuração do Supabase

Configuração ainda não confirmada. Gabriel deve criar/acessar sua conta e criar um projeto individual, por exemplo, `gabriel-251157-caixa-cinza`. Em Authentication, verificar o provedor Email e registrar se a confirmação de e-mail está ativa. Criar um usuário de teste pelo painel e confirmar seu e-mail quando necessário. O objetivo é ter uma identidade válida conhecida para comparar o login correto e as rejeições.

Registrar depois da configuração: nome do projeto, data, URL pública, tipo de chave pública, método de criação do usuário e situação da confirmação de e-mail. Nunca registrar a senha de banco, senha do usuário, chave secreta ou tokens completos.

Identificar Project URL e uma chave pública em Settings/API Keys. Chave da aplicação e access token de sessão têm funções distintas. Usar somente publishable ou anon JWT. Evidências previstas: projeto e identificação, dashboard, Email habilitado, usuário e URL/chave mascarada, conforme checklist abaixo.

# Configuração do Postman

Instalar/abrir o [Postman Desktop](https://www.postman.com/downloads/) e criar um Workspace individual com identificação de Gabriel. Importar os dois arquivos da pasta [postman](postman/) e selecionar o Environment importado. A coleção contém cinco requisições independentes com scripts de verificação. Sua execução real está pendente.

| Variável | Finalidade | Preenchimento |
|---|---|---|
| base_url | Projeto ao qual as chamadas serão enviadas | https://REFERENCIA.supabase.co, sem barra final |
| api_key | Identificar a aplicação | Chave pública do próprio projeto |
| email | Identificar o usuário de teste | E-mail criado e confirmado quando exigido |
| password | Autenticar o usuário | Senha real somente no valor local/privado |

O arquivo de exemplo é entregue vazio. Não exportar o Environment preenchido ao repositório. A marcação secret mascara valores, mas não substitui o cuidado com compartilhamento e sincronização. Registrar versão do Postman e capturar Workspace, Environment, variáveis protegidas e coleção.

# Configuração das Requisições

Método: **POST**. Endpoint: `{{base_url}}/auth/v1/token?grant_type=password`. A chamada solicita uma sessão mediante e-mail e senha.

| Header | Valor |
|---|---|
| apikey | XXXXX (no Postman: {{api_key}}) |
| Content-Type | application/json |
| Authorization | Bearer {{api_key}} somente se api_key for anon JWT legado |

Com chave moderna `sb_publishable_...`, o script remove Authorization e mantém apikey. Essa chave não é JWT e não deve ser usada como Bearer. O enunciado pede os três headers; caso só haja chave moderna, registrar esta adaptação e esclarecer o requisito com o professor. A coleção mantém o header para o caso anon JWT e não usa uma sessão anterior como requisito de login. A configuração efetivamente adotada deve ser documentada com captura dos headers enviados, protegendo os valores.

Body raw JSON do cenário válido:

```json
{"email":"{{email}}","password":"{{password}}"}
```

Exemplo demonstrativo sem dados reais:

```json
{"email":"usuario@example.com","password":"senha-ficticia"}
```

O script serializa as variáveis para preservar caracteres especiais na senha. Os testes não registram nem persistem tokens. Evidências pendentes: método/endpoint, headers e body com senha protegida.

# Execução dos Testes

**Nenhum cenário foi executado ou aprovado nesta preparação.** Antes de executar, confirmar que a senha artificial do CT-002 difere da correta e que o endereço do CT-003 não existe no projeto. Os cinco testes serão enviados pessoalmente por Gabriel, um por vez, usando Send no Desktop.

| Cenário | Entrada Utilizada | Resultado Esperado | Resultado Obtido | Status |
|---|---|---|---|---|
| CT-001 — Login válido | Usuário de teste e senha corretos | HTTP 200 e access_token não vazio | PENDENTE — EXECUTAR NO POSTMAN | PENDENTE |
| CT-002 — Senha incorreta | E-mail existente e senha artificial diferente da correta | Erro de autenticação, sem token | PENDENTE — EXECUTAR NO POSTMAN | PENDENTE |
| CT-003 — Usuário inexistente | E-mail sintaticamente válido, confirmado como não cadastrado | Acesso negado, sem token | PENDENTE — EXECUTAR NO POSTMAN | PENDENTE |
| CT-004 — Campos vazios | email="", password="" | Erro de validação/autenticação, sem token | PENDENTE — EXECUTAR NO POSTMAN | PENDENTE |
| CT-005 — Credenciais inválidas | email="email-invalido" e senha artificial | Erro por credenciais inválidas, sem token | PENDENTE — EXECUTAR NO POSTMAN | PENDENTE |

Os scripts verificam JSON, HTTP e presença/ausência de access_token. Nos negativos, aceitam códigos de rejeição 400, 401, 403 ou 422 como verificação preliminar, sem antecipar o código observado. Uma resposta negativa só aprova o caso se sua causa corresponder ao cenário: chave incorreta, e-mail não confirmado, rate limit, falha de rede ou erro de servidor não comprovam rejeição correta das credenciais.

CT-005 usa formato de e-mail inválido para diferenciar a entrada. A API pode devolver a mesma mensagem genérica dos CT-002/003, evitando revelar se uma conta existe. Não se exige artificialmente uma mensagem diferente.

Para cada CT: registrar entrada sem segredo, data/hora de Brasília, HTTP, campos JSON, mensagem exata sem valores sensíveis, presença de token e print. Em CT-001, mostrar o nome access_token e mascarar o valor, assim como refresh_token.

# Registro dos Testes

A [planilha de registro](planilha/Registro_Testes_Caixa_Cinza.xlsx) tem cinco linhas, com ID, cenário, entrada, esperado, obtido, status, observações, HTTP, JSON, data/hora e caminho da captura. Os resultados estão pendentes. Preencher após execução e manter correspondência com esta tabela.

Classificar **Sucesso** quando o comportamento observado satisfizer o esperado e **Falha** quando houver divergência após confirmar as pré-condições. Erro HTTP esperado em cenário negativo pode representar sucesso do teste. Sem resposta por problema de ambiente, manter pendente e descrever o impedimento. Ainda não há falhas do sistema identificadas por execução.

Checklist de evidências reais a adicionar em prints/ (nomes planejados, arquivos ainda ausentes):

- 01_supabase_projeto.png — projeto criado com identificação real.
- 02_supabase_painel.png — dashboard.
- 03_supabase_auth_email.png — Email e confirmação configurados.
- 04_supabase_usuario.png — usuário válido.
- 05_supabase_api_url_chave.png — URL e tipo de chave, valor protegido.
- 06_identificacao_aluno.png — identificação de Gabriel/RA no ambiente, podendo compartilhar a captura do projeto se já legível.
- 07_postman_workspace.png — Workspace.
- 08_postman_environment.png — Environment selecionado.
- 09_postman_variaveis.png — quatro variáveis, segredos protegidos.
- 10_postman_colecao.png — cinco requisições.
- 11_postman_headers.png — método, endpoint e headers efetivos.
- 12_postman_body.png — body, senha protegida.
- CT001_login_valido.png — CT-001: Login válido, HTTP e JSON legíveis.
- CT002_senha_incorreta.png — CT-002: Senha incorreta, HTTP e JSON legíveis.
- CT003_usuario_inexistente.png — CT-003: Usuário inexistente, HTTP e JSON legíveis.
- CT004_campos_vazios.png — CT-004: Campos vazios, HTTP e JSON legíveis.
- CT005_credenciais_invalidas.png — CT-005: Credenciais inválidas, HTTP e JSON legíveis.
- 13_planilha_resultados.png — planilha preenchida após os testes.

Inserir as imagens e legendas nas seções correspondentes quando recebidas. Não há imagens simuladas ou links de imagem apontando para arquivos inexistentes.

# Resultados Obtidos

Pendente de execução. Ainda não é possível confirmar funcionamento da autenticação, códigos HTTP observados, mensagens, falhas ou taxa de aprovação. Após a coleta, comparar os cinco cenários e vincular CT, entrada, esperado, resultado real, HTTP/JSON e captura. Estes casos cobrem autenticação por senha e não avaliam autorização/RLS, desempenho ou resistência a ataques.

# Conclusão

A conclusão empírica está pendente. Gabriel deverá informar se executou corretamente todos os cenários, se a autenticação funcionou, dificuldades reais e falhas identificadas. Possíveis melhorias deverão decorrer desses resultados, sem afirmar que já foram implementadas. Os testes de caixa cinza permitem relacionar o contrato conhecido da API às respostas observadas e detectar divergências com rastreabilidade.

Referências consultadas em 03/11/2026:

- Daniel Ohata. *UIUX Funcionais — Caixa Cinza, Aula 11*. Material fornecido, slides 14–31. Exemplo didático; o enunciado atual define o escopo.
- Daniel Ohata. *Enunciado da atividade de testes de caixa cinza em API de autenticação*. Texto fornecido por Gabriel.
- Supabase. [API keys](https://supabase.com/docs/guides/getting-started/api-keys) e [Migrating to publishable and secret API keys](https://supabase.com/docs/guides/getting-started/migrating-to-new-api-keys). Base para distinguir apikey e Authorization.

Entrega final: este repositório público, após inclusão e conferência de todas as evidências, resultados e conclusão. Preparar arquivos não substitui a execução individual exigida.
