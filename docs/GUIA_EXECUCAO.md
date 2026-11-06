> Execução concluída em 05 e 06/11/2026: cinco casos aprovados e 24 asserções aprovadas. Este arquivo conserva o roteiro de reprodução e notas históricas. Os resultados estão no README e na planilha.

# Guia de execução individual — Supabase e Postman

> **Uso do guia:** os passos abaixo orientam uma eventual reprodução. A execução documentada já foi concluída no Postman Desktop; não é necessário repeti-la para aplicar correções documentais.

**Atualização da etapa Supabase:** em 05/11/2026, após o login do aluno, foi conferido o projeto já existente Gabriel - Testes Caixa Cinza. O ambiente está Healthy, Email e Confirm email estão habilitados e há um usuário confirmado. As capturas de configuração 01–06 foram feitas durante a inspeção assistida autorizada e estão descritas no README. Não repetir a criação do projeto ou do usuário. Atualização posterior: as credenciais foram verificadas em chamada auxiliar autorizada, e o Postman Desktop foi instalado, autenticado pelo aluno e configurado com workspace restrito, coleção e Environment. Não repetir essas etapas. Na fase de preparação, CT01 ficou aberto com o ambiente selecionado, aguardando o envio. Esse envio foi realizado em 05/11/2026, seguido de CT02–CT05 em 06/11/2026. As variáveis JSON temporárias são preenchidas pelo pré-script durante o envio.

## Etapa A — Supabase
1. Entre em [Supabase Dashboard](https://supabase.com/dashboard). Após entrar na sua conta, selecione uma organização existente e clique **New project**. Se ainda não houver organização, use **New organization**, crie uma organização pessoal no plano **Free** e depois clique **New project**. Preencha **Project name** com `teste-caixa-cinza`, defina a **Database password** somente na plataforma e guarde-a privadamente. Essa senha é do banco e não é a senha do usuário CT01. Em **Region**, escolha uma região próxima, por exemplo São Paulo se disponível. Confira o plano gratuito e clique **Create new project**. Aguarde a inicialização. Se a conta já possuir um projeto dedicado a esta atividade, abra esse projeto em vez de criar outro.
2. Acesse **Authentication > Providers / Sign In > Email**, habilite e registre o estado da confirmação por e-mail.
3. Acesse **Authentication > Users > Add user**, crie usuário de teste com senha forte e confirme-o. Nunca envie senha pelo chat nem inclua senha real nos prints.
4. Abra **Settings > API Keys** e copie para o Postman, localmente, a **chave pública anon legada**, quando disponível, para a configuração com `Authorization: Bearer`. Alternativamente use `sb_publishable_` com a exceção documentada no README; nunca use `service_role`/`sb_secret_`.
5. Registre as evidências 01 a 06, usando a lista `docs/EVIDENCIAS.md`.

## Etapa B — Postman Desktop
1. Crie um **Workspace privado** chamado `Teste Caixa Cinza — Supabase`.
2. No Workspace escolhido, clique **Import** no canto superior esquerdo e use a opção para selecionar arquivos do computador (**Files** ou **Upload Files**, conforme a versão). Selecione `postman/Supabase_Auth_Tests.postman_collection.json` e `postman/Supabase.postman_environment.example.json`. Confirme a importação se a tela solicitar. Confira a coleção com CT01–CT05 em **Collections** e o ambiente em **Environments**. Importe apenas uma vez para evitar coleções duplicadas.
3. Selecione o ambiente `Supabase — Ambiente de Testes` no canto superior direito.
4. Preencha **apenas localmente** `base_url`, `api_key`, `email`, `password`. Não exporte o ambiente preenchido.
5. Abra `CT01 — Login válido`: confira método `POST`, endpoint `/auth/v1/token?grant_type=password`, headers e body.
6. Clique **Send**, confira HTTP, JSON, token presente e a aba **Test Results**. Guarde o print 13 com **todo** o token e refresh_token ocultos.
7. Execute pessoalmente `CT02`, `CT03`, `CT04` e `CT05`, um por vez, conferindo status e erro coerente. Capture 14 a 17.
8. Se o Postman exibir falha, anote **o motivo observado**, sem mudar o resultado para sucesso apenas por ser um caso negativo. Uma resposta 400 com erro certo pode ser caso aprovado; falha de autenticação da chave não valida cenário de senha incorreta.
9. Se desejar, execute a coleção em **Run**, uma iteração, para obter resumo; não substitui a observação individual de cada caso.
10. Preencha `planilha/Registro_Testes.xlsx` com datas, HTTPs, resultados e observações reais; inclua horários somente se registrados. A entrega atual contém datas sem horário exato e a imagem 18 renderizada da planilha preenchida.

## Etapa C — GitHub e README
1. Atualize o nome/RA do aluno e a data real no `README.md`.
2. Substitua `PENDENTE DE EXECUÇÃO` por resultados efetivos **apenas quando houver evidência**.
3. Em cada seção, incorpore capturas usando Markdown como `![CT01 — Login válido](prints/13-ct01-login-valido.png)` após salvar a imagem.
4. Preencha comparação entre esperado/obtido e conclusão, incluindo dificuldades, falhas e melhorias identificadas de verdade.
5. Verifique segredos em **todos** os arquivos e imagens; não publique o Environment preenchido, chaves secretas, senha ou tokens.
6. Publique o repositório no GitHub com visibilidade **Public**. Confira o link sem estar autenticado.
7. Entregue **somente o link do repositório público** conforme o enunciado.

## Primeira confirmação e evidências do projeto

Assim que o projeto terminar de inicializar, capturar a visão do projeto com o nome `teste-caixa-cinza` em `prints/01-supabase-projeto.png` e o painel carregado em `prints/02-supabase-painel.png`. As imagens devem ser reais e legíveis. Não capturar a tela com a senha do banco visível. Confirmar a criação e revisar essas evidências antes de considerar esta etapa concluída. Os próximos prints, 03 a 06, correspondem ao provedor Email, usuário, localização da API com valores protegidos e identificação do responsável, conforme `EVIDENCIAS.md`.

## Referências de interface

Roteiro conferido em 05/11/2026 com a documentação oficial. Os nomes de algumas opções podem variar conforme a versão da interface.

- [Criar um projeto no Supabase](https://supabase.com/docs/guides/getting-started/quickstarts/nextjs#1-create-a-supabase-project).
- [Autenticação por e-mail e senha](https://supabase.com/docs/guides/auth/passwords).
- [Localizar e utilizar chaves](https://supabase.com/docs/guides/getting-started/api-keys).
- [Importar arquivos no Postman](https://learning.postman.com/docs/getting-started/importing-and-exporting/importing-data).

## Se um teste falhar
- HTTP 401 por API key/configuração: corrigir `base_url`/`api_key`, não contar como rejeição válida do CT02/03/04/05.
- Login válido rejeitado: verificar usuário e confirmação de e-mail no Supabase.
- HTTP 429: interromper as repetições, registrar rate limit e reexecutar após liberar, sem ocultar a dificuldade.
- Rastreabilidade do CT03 executado: o pré-script gera um e-mail aleatório de formato válido, e a API confirmou a rejeição. As capturas disponíveis não comprovam diretamente a ausência do endereço no cadastro; em uma futura reprodução, registrar essa consulta se for necessária a comprovação cadastral.
- Erro `email_address_invalid` em CT03: conferir que o e-mail segue sintaxe válida e não existe no projeto; não usar domínios reservados para o cenário de inexistência.
