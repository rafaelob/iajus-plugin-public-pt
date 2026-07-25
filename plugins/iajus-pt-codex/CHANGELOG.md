# Changelog - IAJUS Jurisprudência PT (Codex)

Formato baseado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/).

## [1.1.0] - 2026-07-25

### Removido
- Skill `consultar-legislacao-pt` e toda a promessa de legislação na vitrine (nome, descrição,
  palavras-chave, fontes, prompts sugeridos). O lançamento PT sai **juris-only**: das normas
  portuguesas na base, nenhuma tem dispositivos indexados e apenas 0,50% tem corpo de texto, e não
  existia caminho da pesquisa para o texto (o leitor endereçava por coordenadas brasileiras). As
  três tools correspondentes já tinham saído do perfil `pt` do servidor, o que deixaria esta skill
  a mandar chamar tool inexistente.
- **Não é "em breve".** Voltará quando houver dispositivos PT efetivamente indexados **e** um
  vocabulário de endereçamento português no leitor - as duas condições, nunca só uma.

## [1.0.1] - 2026-07-23

### Alterado
- Marketplace próprio, totalmente separado do Brasil: `github.com/rafaelob/iajus-plugin-public-pt`
  (`codex plugin add iajus-pt@iajus-pt`). Deixa de ser distribuído no marketplace brasileiro.

## [1.0.0] - por publicar

### Adicionado
- Primeiro lançamento do plugin PT (variante Codex).
- Servidor MCP remoto `iajus-pt` (`https://pt.mcp.iajus.com.br/mcp`), autenticado por OAuth 2.1
  (chave `ik_*` como alternativa de canal privado).
- Skills pt-PT: `pesquisar-jurisprudencia-pt`, `consultar-legislacao-pt`, `estado-corpus-pt`.
