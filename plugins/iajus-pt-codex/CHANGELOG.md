# Changelog - IAJUS Jurisprudência PT (Codex)

Formato baseado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/).

## [1.5.4] - 2026-09-19

### Corrigido
- Mesma correção da gémea Claude: ligações de origem deixam de ser rotuladas em bloco como
  oficiais; no Tribunal Constitucional, `dgsi.pt/atco1` é espelho histórico e
  `tribunalconstitucional.pt` é o portal oficial. As skills mantêm-se byte-idênticas.

## [1.5.3] - 2026-09-19

### Adicionado
- Mesma alteração da gémea Claude: logo `iajus.pt/icon-512.png` no bundle e pacote ChatGPT
  do perfil `pt` (8 tools). Skills continuam byte-idênticas entre os dois.

## [1.5.2] - 2026-09-18

### Corrigido
- Mesma alteração da gémea Claude, e os ficheiros das skills continuam byte-idênticos entre os
  dois. `pesquisar-jurisprudencia-pt` e `verificar-citacoes-pt` deixam de congelar contagens de
  AUJ e a fronteira TC-1998 como cobertura; art. 686.º e `uniformizador_valido` mantêm-se.

## [1.5.1] - 2026-09-18

### Corrigido
- Mesma alteração da gémea Claude, e os ficheiros das skills continuam byte-idênticos entre os
  dois. `estado-corpus-pt` reporta apenas os números vivos de `obter_estatisticas_base`; sai o
  censo congelado de 2026-07-28 e o fallback 682→649. Envelope de erro não é ausência; zero de
  vigência não está medido.

## [1.5.0] - 2026-08-16

### Adicionado
- Mesma alteração da gémea Claude: entra o quinto valor do envelope de desfecho,
  `medida_indisponivel` - a fonte respondeu e NÃO traz a medida, um estado que não é avaria e
  não é zero. Com quatro nomes, o caso caía num vizinho que mente: `erro` afirma uma falha que
  não houve, `sem_resultado` afirma um zero MEDIDO que ninguém mediu, e as duas leituras
  chegam ao advogado como «não existe». O nome NÃO entra nas enumerações que atribuem avaria,
  onde a frase ficaria falsa. Os ficheiros das skills continuam byte-idênticos entre os dois.

## [1.4.0] - 2026-07-30

### Adicionado
- Mesma alteração da gémea Claude, e os ficheiros das skills continuam byte-idênticos entre os
  dois plugins de propósito. Passam a ser **quatro** skills pt-PT:
  **`verificar-citacoes-pt`** (veredicto por citação: CONFIRMADA / DIVERGENTE / NÃO LOCALIZADA /
  FORA DE ÂMBITO) e **`fora-de-ambito-pt`** (o que esta superfície não serve, e como responder
  sem citar direito português de memória).
- A skill de citações **não tem eixo de vigência** e a omissão é deliberada: a base não regista
  vigência de acórdão uniformizador, pelo que um veredicto de "ainda em vigor" seria uma
  afirmação sobre o Direito sem substrato. Também não confere normas nem "quem citou quem" - não
  há grafo de citações PT.
- `fora-de-ambito-pt` ensina a ler as duas recusas do servidor - `tool_unavailable_on_profile`
  (com `acervo_consultado: false`) e `familia_recusada`/`familias_fora_de_escopo` - sem as
  converter em "não existe".

### Não portado do plugin brasileiro, e porquê
- **Legislação portuguesa continua FORA**, e nenhuma skill a promete: o perfil `pt` não regista
  tool de legislação nenhuma e exclui a família `legislacao` na camada de chamada. A skill
  brasileira de legislação estadual não teria sequer objeto - Portugal é um Estado unitário.

### Corrigido
- Marcas de português do Brasil no texto publicado: "registra" -> "regista"; "é você que afirma"
  -> "é o assistente que afirma".

### Guards
- O guard do bundle cobre as quatro skills e passa a exigir: conjunto de skills em disco igual ao
  coberto, zero tools de legislação declaradas, leitura dos envelopes de recusa, ausência de
  promessa de vigência e ausência de léxico pt-BR.

## [1.3.0] - 2026-07-29

### Adicionado
- Mesma alteração da gémea Claude, e os ficheiros das skills continuam byte-idênticos entre os
  dois plugins de propósito. **`buscar_qualificada` entra em `allowed-tools`.** A espécie passou
  a ser selector aceite na linha portuguesa em 2026-07-28 e o servidor rolou a 2026-07-29;
  medido contra o servidor vivo nesse dia, `buscar_qualificada(tipo='auj')` devolve AUJ reais do
  `pt_stj`, com `enunciado` e `link_completo` da DGSI.

### Corrigido
- A skill mandava **não** anunciar a consulta aos acórdãos uniformizadores, por não haver tool que
  os listasse ou filtrasse. Era verdade quando foi escrito e deixou de ser.

### Notas de exatidão — leia antes de usar a tool nova
- **Não enumera:** máximo de **10** por chamada (`k=50` devolve 10 na mesma), e `total` conta o
  que voltou, **não** o que existe. Nunca apresentar o que voltou como lista completa.
- **Ler `uniformizador_valido`, e é a nota mais importante.** Das 682 linhas com a espécie AUJ, só
  as **649** do Supremo são AUJ; as outras **33** são acórdãos da Relação de Lisboa que CITAM um
  AUJ e ficaram mal classificados na fonte. O servidor serve-as **marcadas**
  (`uniformizador_valido: false`, `citavel_como_precedente: false`, `aviso_uniformizador` com o
  art. 686.º do CPC) — verificado no servidor vivo em 2026-07-29. Ignorar a marca é o cliente a
  afirmar que a Relação uniformizou jurisprudência. Só as do Supremo: `orgao='pt_stj'`.
- `materia` e `numero` continuam a não selecionar AUJ (medido: 0 e 0).
- Vigência continua **desconhecida** em todos os 682: nunca afirmar vigente, revogado ou superado.
- **O 649 da 1.2.1 está certo**: 649 (Supremo) + 33 (Relação, mal classificados) = 682. Citar 682
  como «acórdãos uniformizadores» inclui 33 que não uniformizaram nada.

## [1.2.1] - 2026-07-28

### Corrigido
- Mesma correção da gémea Claude, e os ficheiros das skills continuam byte-idênticos entre os
  dois plugins de propósito. **A razão publicada em 1.2.0 para `buscar_qualificada` estar fora de
  `allowed-tools` era falsa.** Dizia-se que o tipo português
  (`acordao_uniformizador_jurisprudencia`) não estava nos conjuntos que o leitor aceita; foi
  medido, **está**, e a espécie **nunca foi selector**. O que fecha a porta é o guard exigir
  `numero` ou `materia`: `numero` guarda o identificador documental da DGSI em 682 de 682
  registos e o campo por trás de `materia` está a NULL nos mesmos 682.
- **A lista de tools não muda; muda a razão.** Uma causa falsa manda arrumar o problema errado e
  deixa o certo de pé - e esta arrumava para sempre o único caminho até aos **649 acórdãos
  uniformizadores do STJ**.
- `buscar_qualificada` devolve **erro de contrato explícito**, não vazio: fica fora por ser
  chamada inútil, **não** por mentir, ao contrário de `buscar_por_citacoes` e
  `obter_versoes_qualificada`, cujas tabelas têm zero linhas e cujo vazio afirma algo falso sobre
  o Direito. O servidor deixou de a tratar como beco.

### Notas
- Versão nova porque a 1.2.0 **já estava publicada** com o texto antigo: mesmo número com
  conteúdo diferente faria quem compara versões concluir que tinha a correção sem a ter.

## [1.2.0] - 2026-07-28

### Alterado
- Mesma alteração da gémea Claude, e os ficheiros das skills são byte-idênticos entre os dois
  plugins de propósito: `allowed-tools` reduzido às quatro modalidades que o corpus português
  serve (`buscar_semantica`, `buscar_hibrida`, `buscar_fts`, `buscar_regex`); saem
  `buscar_por_citacoes`, `obter_versoes_qualificada` e `buscar_qualificada`.
  > ⚠️ A razão que esta versão publicou para `buscar_qualificada` era **falsa**, e os três casos
  > foram tratados como se fossem o mesmo, o que também não é verdade. Corrigido em **1.2.1**.
- Filtro por tribunal pelo slug `pt_<x>`, inequívoco por construção. A sigla nua era armadilha
  (`STJ` resolvia para o órgão brasileiro e devolvia zero em silêncio); corrigido na origem em
  2026-07-28 com resolvedor por linha, mas só depois do servidor atualizado - e a skill diz isso
  em vez de prometer o estado futuro. Deduplicar por `link_completo` (grafia dupla de keyspace,
  logo repetição).
- Janelas por tribunal medidas na base, incluindo a fronteira dura do Tribunal Constitucional
  (1983-1998). A tool de estado está degradada e a skill di-lo; `qualificadas` devolve 682 e o
  número publicável são os 649 do STJ (art. 686.º do CPC).

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
