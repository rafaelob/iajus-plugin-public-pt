# Changelog - IAJUS Jurisprudência & Legislação PT

Todas as alterações relevantes deste plugin são registadas aqui. Formato baseado em
[Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/).

## [1.0.1] - 2026-07-23

### Alterado
- O plugin PT passa a viver num marketplace próprio, totalmente separado do Brasil:
  `github.com/rafaelob/iajus-plugin-public-pt` (instalação `iajus-pt@iajus-pt`). Deixa de
  ser distribuído no marketplace brasileiro `iajus-plugin-public`, onde a coexistência
  fazia o assistente de utilizadores BR assumir a identidade portuguesa.

## [1.0.0] - por publicar

### Adicionado
- Primeiro lançamento do plugin PT.
- Servidor MCP remoto `iajus-pt` (`https://pt.mcp.iajus.com.br/mcp`), autenticado por OAuth 2.1
  (chave `ik_*` como alternativa de canal privado).
- Skill `pesquisar-jurisprudencia-pt`: jurisprudência PT via DGSI (STJ, Tribunais da Relação,
  STA, TCA-Sul/Norte, Tribunal Constitucional, Conflitos, Tribunal de Contas); modalidades
  semântica, híbrida, texto integral, regex, citações, qualificadas e acórdão uniformizador.
- Skill `consultar-legislacao-pt`: legislação do Diário da República por nome e por número,
  texto integral, ELI, vigência e caducidade.
- Skill `estado-corpus-pt`: estado do corpus PT ao vivo (por órgão, ano e família).
