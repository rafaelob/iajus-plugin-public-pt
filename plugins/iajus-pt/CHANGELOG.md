# Changelog - IAJUS Jurisprudência PT

Todas as alterações relevantes deste plugin são registadas aqui. Formato baseado em
[Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/).

## [1.5.4] - 2026-09-19

### Corrigido
- Ligações de origem deixam de ser rotuladas em bloco como oficiais. Para resultados do
  Tribunal Constitucional, `dgsi.pt/atco1` é identificado como espelho histórico e
  `tribunalconstitucional.pt` como portal oficial. A promessa pública, os exemplos e as skills
  conservam a proveniência sem alterar tools, permissões ou a submissão reservada ao operador.

## [1.5.3] - 2026-09-19

### Adicionado
- Logo servida em `iajus.pt/icon-512.png` no bundle (`assets/iajus-pt-icon-512.png`) e pacote
  de submissão ChatGPT (`saas/clients/plugin_pt/`: 8 tools do perfil `pt`, MCP
  `https://pt.mcp.iajus.com.br/mcp`, ZIPs das quatro skills). Não é o plugin brasileiro de 28
  tools. Publicação no marketplace e no Plugin Directory da OpenAI continua a ser do ARQ.

## [1.5.2] - 2026-09-18

### Corrigido
- `pesquisar-jurisprudencia-pt` e `verificar-citacoes-pt` deixam de congelar contagens de AUJ
  (649/682/33) e a fronteira TC-1998 como cobertura. Só o STJ uniformiza (art. 686.º CPC);
  `uniformizador_valido` continua a marcar as linhas da Relação mal classificadas. Um vazio
  filtrado ao TC com `desfecho=sem_resultado` é zero desta chamada, não censo.

## [1.5.1] - 2026-09-18

### Corrigido
- `estado-corpus-pt` reporta **apenas os números vivos** de `obter_estatisticas_base`. Sai o
  censo congelado de 2026-07-28 (331.045 acórdãos / 12 tribunais / tabela de anos, incluindo a
  fronteira TC 1983-1998) e o fallback 682→649. Um envelope de erro **não é ausência**; zero num
  campo de vigência **não está medido**. A skill não preenche cobertura a partir da memória nem
  de qualquer censo gravado nela.

## [1.5.0] - 2026-08-16

### Adicionado

- **Quinto valor do envelope de desfecho: `medida_indisponivel`.** As skills passam a ensinar
  que a fonte pode responder e NÃO trazer a medida - um estado que não é avaria e não é zero.
  Com quatro nomes apenas, este caso caía no mais próximo, e os dois vizinhos mentem em
  sentidos opostos: `erro` afirma uma falha que não houve, e `sem_resultado` afirma um zero
  MEDIDO que ninguém mediu. As duas leituras chegam ao advogado com a mesma frase - «não
  existe» - e é para isso que o quinto nome serve.
- O nome entra nas enumerações que dizem o que NÃO é medição. **Não** entra nas que atribuem
  avaria («reporte a falha», «não verificável por falha»): ali a frase ficaria falsa, porque
  `medida_indisponivel` é não-medido SEM avaria.

## [1.4.0] - 2026-07-30

### Adicionado
- **`verificar-citacoes-pt`** - confere uma a uma as citações de jurisprudência portuguesa de uma
  peça contra a fonte, com veredicto por citação: CONFIRMADA / DIVERGENTE / NÃO LOCALIZADA /
  FORA DE ÂMBITO. É o homólogo português da skill brasileira de conferência, mas **estreitado ao
  que este perfil serve de facto**, e as amputações são a parte importante: não há eixo de
  VIGÊNCIA (a base não a regista - `status_vigencia` vem "desconhecida" e um zero ali é não
  medido, nunca "nenhum vigente"), não há conferência de normas, e não há confirmação de que um
  acórdão posterior acolheu o precedente (não existe grafo de citações PT). Um veredicto é uma
  afirmação sobre o Direito: emitir "ainda em vigor" a partir de um campo não medido seria
  exatamente o defeito que a skill existe para apanhar.
- **`fora-de-ambito-pt`** - responde ao que esta superfície NÃO serve. Existe porque a resposta
  errada a um pedido de legislação portuguesa não é "não sei": é o assistente **citar um artigo
  de memória** para não ficar de mãos vazias. A skill proíbe-o em termos duros, encaminha para o
  Diário da República e entrega o que a base tem mesmo - a jurisprudência que aplica a norma,
  achada por `buscar_fts`/`buscar_regex`. Ensina também a **ler as duas recusas do servidor** sem
  as converter em mentira: `error_code: "tool_unavailable_on_profile"` (com `acervo_consultado:
  false`, e sem `resultados` nem `total`, de propósito) e `familia_recusada`/
  `familias_fora_de_escopo`. Nenhuma das duas é uma afirmação sobre o Direito.

### Não portado do plugin brasileiro, e porquê
- **`consultar-legislacao` e `consultar-legislacao-estadual` NÃO têm homólogo PT, e não é
  omissão.** O perfil `pt` do servidor não regista uma única tool de legislação, e ainda exclui a
  família `legislacao` na camada de chamada - porque as tools de leitura só endereçam por
  coordenadas federativas brasileiras e a esmagadora maioria das normas PT não tem corpo de texto
  indexado. Uma skill que prometesse legislação portuguesa devolveria vazio ou um beco, e ambos se
  leem como uma afirmação falsa sobre o Direito. A segunda não teria sequer objeto: Portugal é um
  Estado unitário e não tem a camada estadual/municipal que a skill brasileira serve. O que ficou
  no lugar delas é `fora-de-ambito-pt`, que declara a ausência em vez de a encenar.
- **`verificar-citacoes` foi portado com menos eixos** (ver acima), e não com os mesmos: o eixo de
  vigência e a conferência de dispositivos legais não têm substrato nesta linha.

### Corrigido
- Duas marcas de português do Brasil no texto publicado: "não **registra** vigência" passa a "não
  **regista** vigência" (`pesquisar-jurisprudencia-pt` e `estado-corpus-pt`), e "é **você** que
  afirma" passa a "é **o assistente** que afirma". O bundle é pt-PT europeu; o registo é
  impessoal.

### Guards
- `tests/saas/test_plugin_public_pt_bundle.py` passa a cobrir as quatro skills e ganha guards
  novos: o **conjunto de skills em disco** tem de ser exatamente o que o ficheiro cobre (uma
  skill nova deixava de ter guard nenhum, em silêncio); **nenhuma skill pode declarar uma tool de
  legislação**, e o ledger dessas tools tem de continuar disjunto de `PT_TOOLS` (se a legislação
  voltar ao perfil, o guard acende e obriga a uma decisão em vez de deixar a skill desatualizada
  a decidir sozinha); a skill de âmbito tem de ensinar a ler os dois envelopes de recusa; a de
  citações não pode prometer veredicto de vigência; e nenhuma skill pode usar léxico de português
  do Brasil nem nomear instituições de outro ordenamento.

## [1.3.0] - 2026-07-29

### Adicionado
- **`buscar_qualificada` entra em `allowed-tools`: a porta até aos acórdãos uniformizadores
  abriu.** A 1.2.1 explicou corretamente porque estava fechada — a espécie nunca fora selector,
  e o guard exigia `numero` ou `materia`, que o dado português fecha. Em 2026-07-28 a espécie
  passou a ser selector aceite na linha portuguesa, e o servidor que a serve rolou a 2026-07-29.
  **Medido contra o servidor vivo nesse dia:** `buscar_qualificada(tipo='auj')` devolve AUJ reais
  do `pt_stj`, com `enunciado` e `link_completo` da DGSI. Aceita a alcunha `'auj'` ou a espécie
  por extenso, mais `orgao` e `k`.

### Corrigido
- **A skill dizia «não há tool que liste ou filtre os AUJ» e «não anuncie que vai consultar os
  acórdãos uniformizadores».** Era verdade quando foi escrito e deixou de ser. Uma instrução que
  manda o assistente não procurar aquilo que já pode encontrar é, para o utilizador,
  indistinguível de o acervo não existir.

### Notas de exatidão — leia antes de usar a tool nova
- **`buscar_qualificada` NÃO enumera o acervo.** Devolve no máximo **10** por chamada (`k=50`
  continua a devolver 10) e o campo `total` do envelope conta **o que voltou, não o que existe**.
  Apresentar o que voltou como a lista completa, ou contar AUJ a partir desse `total`, é afirmar
  algo falso sobre o Direito português. A skill carrega este aviso e o bundle tem guard que falha
  se ele desaparecer.
- **A skill passa a mandar ler `uniformizador_valido`, e é a nota mais importante desta versão.**
  A tabela tem **682** linhas com a espécie AUJ, mas só as **649** do Supremo são AUJ: as outras
  **33** são acórdãos da Relação de Lisboa que CITAM ou discutem um AUJ e ficaram com a espécie
  errada por defeito de classificação da fonte (já registado, e o dono é o agente do corpus). O
  servidor **não as esconde nem mente** — serve-as marcadas com `uniformizador_valido: false`,
  `citavel_como_precedente: false` e um `aviso_uniformizador` que invoca o art. 686.º do CPC
  (verificado no servidor vivo em 2026-07-29). O servidor faz a metade dele; **a metade do cliente
  é não ignorar a marca**, e um assistente que a ignore afirma que a Relação uniformizou
  jurisprudência. Para receber só as do Supremo: `orgao='pt_stj'`.
- **`materia` e `numero` continuam a não selecionar AUJ** (medido: `materia='civil'` → 0,
  `numero='8/2022'` → 0), pelas mesmas razões documentadas em 1.2.1. Para chegar ao AUJ n.º 8/2022
  pelo número, pesquise-o pelas modalidades de texto.
- **A vigência continua desconhecida**: todos vêm com `status_vigencia: "desconhecida"`, e a
  estatística conta 0 vigentes e 0 canceladas em 682. Nunca afirme que um AUJ está vigente,
  revogado ou superado.
- **O 649 da 1.2.1 está certo e continua a ser o número publicável.** A leitura crua da base viva
  devolve 682 para a espécie, o que parece contradizê-lo e não contradiz: 649 são os AUJ do
  Supremo e 33 são as linhas mal classificadas da Relação. **649 + 33 = 682.** Quem citar o 682
  como «acórdãos uniformizadores portugueses» inclui 33 acórdãos que não uniformizaram nada.

## [1.2.1] - 2026-07-28

### Corrigido
- **A razão publicada em 1.2.0 para `buscar_qualificada` estar fora de `allowed-tools` era
  falsa, e esta versão existe para a corrigir.** A 1.2.0 afirmava que «o tipo português
  (`acordao_uniformizador_jurisprudencia`) não é um dos valores que a tool aceita». Foi medido:
  **é**, consta dos conjuntos do leitor, e a espécie **nunca foi selector** - portanto não podia
  ser causa de coisa nenhuma. O que de facto fecha a porta é o guard de selectores exigir
  `numero` ou `materia`, e o dado fechar os dois: `numero` guarda o identificador documental da
  DGSI (um hash, não um número de acórdão citável) em 682 de 682 registos, e o campo por trás de
  `materia` está a NULL nos mesmos 682.
- **A lista de tools não muda; muda a razão** - e a razão é o que importa aqui. Uma causa falsa
  não é um detalhe de redação: manda arrumar um problema que não existe e deixa o verdadeiro
  intacto, e esta em particular dizia que a espécie portuguesa não é aceite, o que arruma para
  sempre o único caminho até aos **649 acórdãos uniformizadores do STJ**. Enquanto a causa
  declarada for a errada, ninguém vai abrir a porta certa.
- `buscar_qualificada` responde hoje **erro de contrato explícito** (`indique 'numero' ou
  'materia'`), não vazio. Fica fora da skill por ser chamada inútil, **não** por mentir - ao
  contrário de `buscar_por_citacoes` e `obter_versoes_qualificada`, que lêem tabelas com zero
  linhas e cujo vazio afirma algo falso sobre o Direito. A mesma distinção foi aplicada no
  servidor, que deixou de a tratar como beco.

### Notas
- **Porquê uma versão nova para uma correção de texto:** a 1.2.0 **já estava publicada** com o
  texto antigo. Corrigir o conteúdo mantendo o número faria duas coisas diferentes chamarem-se
  1.2.0, e quem comparasse versões concluiria que tinha a versão corrigida sem a ter. Uma
  reparação invisível é pior do que a avaria que corrige.

## [1.2.0] - 2026-07-28

### Alterado
- As duas skills passaram a declarar em `allowed-tools` **apenas as tools que o corpus português
  consegue servir**: `buscar_semantica`, `buscar_hibrida`, `buscar_fts` e `buscar_regex`. Saíram
  `buscar_por_citacoes`, `obter_versoes_qualificada` e `buscar_qualificada`.
  > ⚠️ A razão que esta versão publicou para `buscar_qualificada` era **falsa**, e os três casos
  > foram tratados como se fossem a mesma coisa, o que também não é verdade. Corrigido em
  > **1.2.1** - ver acima.
- **Filtro por tribunal: usar o slug `pt_<x>`**, que é inequívoco por construção e alcança o órgão
  inteiro (medido: cada slug chega a 100% dos acórdãos distintos do seu tribunal). A sigla nua
  depende de o servidor saber que linha serve, e era armadilha: `STJ` resolvia com sucesso para o
  Superior Tribunal de Justiça **brasileiro** e devolvia **zero em silêncio** - e um zero calado
  lê-se como «não há jurisprudência do Supremo». Corrigido na origem em 2026-07-28 (resolvedor por
  linha; sigla presente nas duas linhas devolve ambiguidade em vez de eleger o Brasil), **mas só
  vale depois do servidor atualizado**, e a skill di-lo em vez de prometer o estado futuro. Há
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
