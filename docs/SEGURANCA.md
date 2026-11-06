# Conferência de privacidade e integridade

Revisão local em 06/11/2026, antes da publicação.

- Environment de exemplo contém apenas placeholders nas quatro variáveis. Ambiente real não foi exportado.
- Senha real, e-mail do usuário de teste, JWTs completos e chaves reais não foram encontrados nos textos e XMLs dos arquivos entregues.
- Capturas do usuário e da API foram revisadas visualmente; identificadores desnecessários e chave pública foram ocultados com preenchimento opaco.
- Os valores integrais de access_token e refresh_token do CT01 foram ocultados na cópia pública. O token não foi salvo na coleção ou na planilha.
- Capturas 14–17 foram recortadas apenas para excluir o painel lateral de sugestões do aplicativo. A requisição, o HTTP, o JSON e as asserções permanecem visíveis.
- Capturas originais com dados sensíveis e scripts auxiliares ficam fora da pasta da entrega e fora do pacote ZIP.
- Foram conferidos os links relativos do README, os dois JSON importáveis, as quatro fórmulas do resumo e a validação de status da planilha.
- Resultado: cinco cenários com Sucesso; HTTPs 200, 400, 400, 400 e 400. Resumo calculado: cinco aprovados, zero falhas, zero bloqueados e zero pendentes.
- O .gitignore protege ambientes locais e arquivos privados comuns, mas não substitui a inspeção dos arquivos e das imagens.

Após autorização do aluno, a versão final foi publicada no repositório teste-caixa-cinza-UX-UI. Os 34 arquivos publicados foram comparados ao pacote local por SHA Git; repositório e README responderam HTTP 200 em requisições sem credenciais. O histórico foi preservado e os três arquivos antigos de preparação foram substituídos pelos arquivos revisados, sem manter versões duplicadas na árvore atual. A revisão não abrange arquivos externos a este pacote ou credenciais eventualmente publicadas em outros locais.
