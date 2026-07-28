# Changelog - IAJUS Jurisprudência PT

Todas as alterações relevantes deste plugin são registadas aqui. Formato baseado em
[Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/).

## [1.2.0] - 2026-07-28

### Alterado
- As duas skills passaram a declarar em `allowed-tools` **apenas as tools que o corpus português
  consegue servir**: `buscar_semantica`, `buscar_hibrida`, `buscar_fts` e `buscar_regex`. Saíram
  três que estavam declaradas e são becos medidos na base PT - `buscar_qualificada` (sem porta de
  entrada: `numero` é um hash do DGSI, `ramo_hint` é NULL em 682 de 682 registos e o tipo PT não
  está nos conjuntos que o leitor aceita), `buscar_por_citacoes` e `obter_versoes_qualificada`
  (tabelas com zero linhas). Uma tool declarada que devolve vazio **não é «sem resultados»: é uma
  afirmação falsa sobre o Direito** - um `buscar_por_citacoes` vazio lê-se como «ninguém citou
  este acórdão», e um `obter_versoes_qualificada` vazio como «a redação nunca mudou».
- **Filtro por tribunal: usar o slug `pt_<x>`**, que é inequívoco por construção e alcança o órgão
  inteiro (medido: cada slug chega a 100% dos acórdãos distintos do seu tribunal). A sigla nua
  depende de o servidor saber que linha serve, e era armadilha: `STJ` resolvia com sucesso para o
  Superior Tribunal de Justiça **brasileiro** e devolvia **zero em silêncio** - e um zero calado
  lê-se como «não há jurisprudência do Supremo». Corrigido na origem em 2026-07-28 (resolvedor por
  linha; sigla presente nas duas linhas devolve ambiguidade em vez de eleger o Brasil), **mas só
  vale depois do servidor actualizado**, e a skill di-lo em vez de prometer o estado futuro. Há
  grafia dupla de keyspace na base, pelo que o mesmo acórdão pode voltar duas vezes: deduplicar
  por `link_completo`.
- Janelas temporais por tribunal **medidas** na base, uma a uma, com a fronteira dura do
  **Tribunal Constitucional (1983-1998)** dita como fronteira e não como «cobertura em
  andamento». Três das quatro janelas que antes vinham por dedução estavam erradas.
- `estado-corpus-pt`: a tool de estado está degradada (as secções `familias`/`orgaos`/`tudo`
  devolvem só um envelope de erro), e a skill di-lo. `qualificadas` é a única secção fiável - e
  devolve **682**, dos quais 33 são de uma Relação, que não pode uniformizar jurisprudência
  (art. 686.º do CPC): o número publicável são os **649** do STJ. `vigentes: 0` é tratado como
  **não medido**, nunca como «nenhum vigente».

## [1.1.0] - 2026-07-25

### Removido
- Skill `consultar-legislacao-pt` e toda a promessa de legislação na vitrine (nome, descrição,
  palavras-chave, fontes). O lançamento PT sai **juris-only**: das normas portuguesas na base,
  nenhuma tem dispositivos indexados e apenas 0,50% tem corpo de texto, e não existia caminho da
  pesquisa para o texto (o leitor endereçava por coordenadas brasileiras). A superfície devolveria
  vazio, e vazio parece defeito. As três tools correspondentes já tinham saído do perfil `pt` do
  servidor, o que deixaria esta skill a mandar chamar tool inexistente.
- **Não é "em breve".** Voltará quando houver dispositivos PT efetivamente indexados **e** um
  vocabulário de endereçamento português no leitor - as duas condições, nunca só uma.

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
