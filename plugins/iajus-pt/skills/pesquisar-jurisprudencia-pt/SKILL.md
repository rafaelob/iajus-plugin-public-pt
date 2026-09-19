---
name: pesquisar-jurisprudencia-pt
description: Pesquisa e cita jurisprudência portuguesa real (Supremo Tribunal de Justiça, Tribunais da Relação, STA, TCA-Sul/Norte, Tribunal Constitucional, Conflitos, Tribunal de Contas) pelo MCP IAJUS - modalidades semântica, híbrida, texto integral e regex. Acione para acórdão, descritor, sumário, ECLI, tese firmada ou entendimento de um tribunal português.
allowed-tools: mcp__iajus-pt__buscar_semantica, mcp__plugin_iajus-pt_iajus-pt__buscar_semantica, mcp__iajus-pt__buscar_hibrida, mcp__plugin_iajus-pt_iajus-pt__buscar_hibrida, mcp__iajus-pt__buscar_fts, mcp__plugin_iajus-pt_iajus-pt__buscar_fts, mcp__iajus-pt__buscar_regex, mcp__plugin_iajus-pt_iajus-pt__buscar_regex, mcp__iajus-pt__buscar_qualificada, mcp__plugin_iajus-pt_iajus-pt__buscar_qualificada
---

# Pesquisar jurisprudência portuguesa (IAJUS)

Tem acesso ao servidor MCP `iajus-pt`, que indexa jurisprudência portuguesa dos tribunais
superiores e das Relações, recolhida da fonte oficial DGSI (www.dgsi.pt), mais o Tribunal de
Contas. **Use o MCP em vez de inventar jurisprudência - a fonte é a verdade; nunca cite de
memória.**

> **Corpus VIVO e em crescimento:** a base é ingerida continuamente; órgãos e anos novos
> aparecem na pesquisa automaticamente, sem alteração de skill. Leia `desfecho` ANTES
> de qualquer contagem: `erro`/`nao_terminou`/`medida_indisponivel` = a consulta não mediu; só
> `sem_resultado` é zero MEDIDO. Um vazio filtrado não é censo nem fronteira permanente.

## Envelope de desfecho

Leia a chave `desfecho` ANTES de qualquer contagem. Os cinco valores são mutuamente exclusivos:

- `erro` — a consulta FALHOU; ninguém consultou o acervo. Não é ausência.
- `sem_resultado` — a consulta CORREU e o acervo não tem. Zero MEDIDO.
- `nao_terminou` — tempo esgotado ou tecto. NÃO-MEDIDO; não afirme que «não existe».
- `parcial` — mediu uma parte; declare o que ficou de fora.
- `medida_indisponivel` - a fonte respondeu e NÃO carrega a medida. NÃO-MEDIDO sem avaria; não é zero.

`total: 0` só é ausência medida quando `desfecho` é `sem_resultado`. Sem `desfecho`, ou com `erro`/`nao_terminou`/`medida_indisponivel`, diga que a consulta não mediu.

## Órgãos FILTRÁVEIS (DGSI + Tribunal de Contas)

Janela **interpretativa, não censo; confirme na tool**. Serve para interpretar um vazio nesta
chamada, não para afirmar cobertura. O slug serve para filtrar (`orgao_code`), sempre na forma
`pt_<x>`, que é inequívoca - ver as notas de uso.

> ⚠️ **Esta é a lista do que se pode FILTRAR, não a lista do que a base contém.** Medido em
> 2026-07-28: o acervo serve ainda unidades do Tribunal de Justiça da União Europeia
> (guardadas sob `CJUE`) e do Tribunal Europeu dos Direitos Humanos (`TEDH`), e o filtro
> por tribunal **não as alcança** - o código está guardado em maiúsculas e o filtro liga a forma
> em minúsculas, que casa zero linhas. Duas consequências para quem lê: **não prometa filtrar por
> TJUE ou TEDH**, porque não funciona; e **um vazio sob filtro nunca prova ausência** destas duas
> jurisdições. Não são tribunais portugueses.

| Tribunal | Slug | Janela (interpretativa) | Nota |
|---|---|---|---|
| Supremo Tribunal de Justiça | `pt_stj` | 1994-2026 | Supremo comum; profere acórdão uniformizador de jurisprudência (AUJ). |
| Tribunal da Relação de Lisboa | `pt_trl` | 1992-2026 | 2.ª instância. |
| Tribunal da Relação do Porto | `pt_trp` | 1994-2026 | 2.ª instância. |
| Tribunal da Relação de Coimbra | `pt_trc` | 1965-2026 | 2.ª instância; a série mais antiga da base. **Pode repetir resultados**. |
| Tribunal da Relação de Évora | `pt_tre` | 1998-2026 | 2.ª instância. **Pode repetir resultados** (ver notas). |
| Tribunal da Relação de Guimarães | `pt_trg` | 2002-2026 | 2.ª instância. **Pode repetir resultados**. |
| Supremo Tribunal Administrativo | `pt_sta` | 1950-2026 | Cúpula da jurisdição administrativa e fiscal. |
| Tribunal Central Administrativo Sul | `pt_tcas` | 1997-2026 | Administrativo e fiscal, 2.ª instância. |
| Tribunal Central Administrativo Norte | `pt_tcan` | 2004-2026 | Administrativo e fiscal, 2.ª instância. **Pode repetir resultados**. |
| Tribunal Constitucional | `pt_tc` | confirme na tool | Fiscalização da constitucionalidade (Art. 281.º CRP). |
| Conflitos | `pt_conflitos` | 1969-2026 | Conflitos de jurisdição/competência. **Pode repetir resultados**. |
| Tribunal de Contas | `pt_tdc` | 2001-2026 | Fiscalização prévia e concomitante das contas públicas. |

> **Tribunal Constitucional: um vazio nesta chamada não é fronteira permanente.** Se uma
> consulta filtrada a `pt_tc` devolver `desfecho=sem_resultado`, essa chamada mediu zero; não
> declare que a base termina num ano nem invente um corte temporal. Para uma questão de
> constitucionalidade recente que a tool não devolveu, remeta o utilizador ao sítio oficial
> (www.tribunalconstitucional.pt) — é reenvio à fonte, não um censo.

## Escolha da modalidade

Comece pela modalidade certa para a pergunta. As pesquisas devolvem um envelope uniforme
(`{ modalidade, total, resultados:[...] }`) e são somente-leitura.

| Pergunta do utilizador | Tool | Porquê |
|---|---|---|
| Tema/conceito ("responsabilidade civil por acidente de viação") | `buscar_semantica` | Vetorial/densa - casa por significado. **Padrão para perguntas conceituais.** |
| Melhor resultado geral (relevância máxima) | `buscar_hibrida` | Funde semântica e texto integral por RRF. **Recorra a ela quando a densa vier fraca.** |
| Expressão exata / termo técnico literal ("enriquecimento sem causa") | `buscar_fts` | Texto integral, insensível a acento; procura a expressão tal como escrita. |
| Padrão literal / forma de citação ("Artigo 483.º", um número de ECLI) | `buscar_regex` | Expressão regular; inclua um trecho literal com pelo menos 3 caracteres. |

Notas de uso:

- **Filtro por tribunal: use o slug `pt_<x>`.** É a forma inequívoca por construção - liga
  directamente à coluna `orgao_code` do corpus e vale seja qual for a versão do servidor. A sigla
  nua (`STJ`, `TRL`, `TC`) depende de o servidor saber que linha de produto serve, e é aí que
  morava uma armadilha: enquanto o vocabulário de órgãos era só brasileiro, `STJ` resolvia-se com
  SUCESSO para o **Superior Tribunal de Justiça brasileiro** e devolvia **zero em silêncio**, sem
  erro - o que se lê como "não há jurisprudência do Supremo", uma afirmação falsa sobre o Direito.
  Corrigido na origem em 2026-07-28 (o resolvedor passou a ser por linha: em Portugal `STJ`
  resolve para `pt_stj`, e uma sigla que exista nas DUAS linhas devolve ambiguidade em vez de
  eleger o Brasil). Escreva o slug e a questão não se põe.
- **O slug alcança o tribunal inteiro; o que a grafia dupla estraga é a repetição.** Cinco
  órgãos estão guardados sob duas grafias (`pt_<x>` e `<x>_pt`), mas medido em 2026-07-28 o
  filtro `pt_<x>` alcança **todos** os acórdãos distintos de cada um deles. O efeito real é
  outro: **o mesmo acórdão pode voltar mais de uma vez** na mesma lista de resultados. Antes de
  apresentar, deduplique pelo `link_completo` e não confunda duas ocorrências com dois
  precedentes. Defeito conhecido, em correção no servidor.
- **Recorte por ano:** as modalidades aceitam ano ou faixa (`ano_min`/`ano_max`). **Confirme
  sempre a data do próprio resultado** antes de a citar, em vez de assumir que o filtro a
  garantiu.
- `buscar_regex` recusa padrões só de metacaracteres; inclua um trecho literal com pelo menos
  3 caracteres.

## Acórdãos uniformizadores de jurisprudência (AUJ)

Só o Supremo Tribunal de Justiça uniformiza jurisprudência (art. 686.º CPC): um acórdão de Relação nunca é AUJ, por muito que cite um. Cite pelo teor. O número que existe é o que a tool devolver nesta chamada, não um censo gravado aqui.

**Como chegar lá, e há duas portas:**

1. **`buscar_qualificada(tipo='auj')`** - devolve AUJ com `enunciado` e `link_completo` da DGSI.
   Aceita a alcunha `'auj'` ou a espécie por extenso `'acordao_uniformizador_jurisprudencia'`, e
   opcionalmente `orgao='pt_stj'` e `k`.
2. **As modalidades de pesquisa acima** - um AUJ está indexado com o mesmo texto e o mesmo
   `link_completo` que qualquer acórdão. Pesquise a tese ("uniformização de jurisprudência" mais
   o tema, ou só o tema) e leia o que volta.

**Use as DUAS.** A primeira encontra AUJ pela espécie mas **não devolve o conjunto** (ver abaixo);
a segunda encontra pelo tema mas não distingue o AUJ do acórdão comum que o cita.

**O que NÃO existe hoje - e não o encene:**

- **`buscar_qualificada` NÃO enumera o acervo.** Devolve no máximo **10** por chamada, e `k=50`
  continua a devolver 10. O campo `total` do envelope conta **o que voltou, não o que existe**.
  **Nunca** apresente o que voltou como a lista completa, e **nunca** conte AUJ a partir deste
  `total` - é a diferença entre informar e afirmar algo falso sobre o Direito português.
- **Passe `orgao='pt_stj'`. É o filtro que separa, e é o único.** Só o Supremo uniformiza
  (art. 686.º CPC). Há linhas com a espécie AUJ que são acórdãos da Relação de Lisboa que CITAM
  ou discutem um AUJ e ficaram com a espécie errada por defeito de classificação da fonte. Se
  apresentar uma dessas como acórdão uniformizador, é o assistente que afirma que a Relação
  uniformizou jurisprudência - e é falso.
- **Leia `uniformizador_valido` antes de tratar um resultado como AUJ.** O servidor não esconde
  nem mente - serve as linhas mal classificadas da Relação marcadas com
  `uniformizador_valido: false` e um `aviso_uniformizador`. Mas leia com cuidado o que cada
  marca significa:
  - `uniformizador_valido` **só existe na linha defeituosa**. Nas linhas boas a chave está
    **AUSENTE**, nunca `true`. Ausência aqui é ausência de objeção, nunca um atestado - quem
    exigir `uniformizador_valido == true` não encontra nenhuma e conclui que não há AUJ nenhum.
  - `citavel_como_precedente` vem **`false` também nas linhas boas**. Não é a marca da Relação
    mal classificada: é consequência de a vigência não estar registada (ver o ponto seguinte),
    e o campo exige vigência **em vigor** para sair `true`. Como discriminador de espécie, não
    serve.
- **`materia` e `numero` não selecionam AUJ.** `materia='civil'` devolve 0 e `numero='8/2022'`
  devolve 0, porque o campo por trás de `materia` está a NULL nestes registos e `numero` guarda
  o identificador documental da DGSI (um hash), não o número citável do acórdão. Para chegar ao
  **AUJ n.º 8/2022 pelo número**, pesquise-o pelas modalidades de texto, não por `numero`.
- Uma pesquisa por "uniformização" **não devolve o conjunto**: só parte dos AUJ usa
  essa palavra no sumário. Não conclua a partir do que voltou que os restantes não existem.
- A base **não regista vigência** de AUJ - vêm com `status_vigencia: "desconhecida"`. Zero em
  vigentes/canceladas é **não medido**. **Nunca** afirme que um está vigente, revogado ou
  superado; cite pelo teor e remeta a confirmação à fonte oficial (DGSI).

## Método do pesquisador (4 passos)

Uma pesquisa jurídica a sério quase nunca é UMA chamada: é uma varredura que escala de
modalidade até a cobertura estabilizar.

1. **Escale a modalidade em vez de cair no vazio.** Uma pesquisa com
   `desfecho=sem_resultado` (ou hits cujo trecho não responde) NÃO é sinal de
   parar — é sinal de escalar. Se `desfecho` for `erro` ou `nao_terminou`,
   reporte a falha e não escale como se o acervo estivesse vazio. Nesta ordem:
   - `buscar_semantica` -> **`buscar_hibrida`** com a MESMA consulta (a fusão resgata o que a
     densa isolada perdeu);
   - **reformule** com os termos que apareceram nos primeiros hits (relator, descritor,
     dispositivo citado, número de acórdão);
   - **troque de modalidade pela forma da pergunta:** termo técnico literal -> `buscar_fts`;
     citação de artigo/ECLI -> `buscar_regex`;
   - **suba na hierarquia de autoridade:** se uma Relação vier vazia para a tese, tente o
     Supremo competente (STJ para o comum, STA para o administrativo/fiscal) - a tese firmada
     costuma estar lá. **Para constitucionalidade, um vazio em `pt_tc` com
     `desfecho=sem_resultado` é zero desta chamada, não uma fronteira permanente.**
2. **Prefira o consolidado ao acórdão isolado.** Para "qual o entendimento atual", um AUJ do
   Supremo vale mais que um acórdão de Relação; se um AUJ aparecer nos resultados, ancore nele
   e use os acórdãos comuns para exemplificar a aplicação da tese.
3. **Refine até a cobertura estabilizar** (rondas sem resultado novo relevante) e cruze as
   modalidades: densa/híbrida para o panorama, FTS/regex para termos e citações literais.
4. **Envelope de honestidade.** Leia `desfecho` primeiro: `sem_resultado` é zero
   MEDIDO (cobertura em andamento, não «o precedente não existe» — ofereça a fonte
   superior); `erro`/`nao_terminou`/`medida_indisponivel` é NÃO-MEDIDO; `parcial` declara o que ficou de
   fora. Se a consulta ao TC devolver `sem_resultado`, essa chamada mediu zero; remeta à
   fonte oficial se a questão for recente e a tool não a devolveu. Nunca preencha a lacuna com um
   precedente fabricado.

## Passada de conferência anti-alucinação (obrigatória antes de entregar)

Toda citação que entregar tem de ter vindo de uma chamada REAL nesta sessão, com o
`link_completo` que a fonte devolveu. Antes de fechar, confira cada citação:

- **existe?** o acórdão com aquele número/descritor/ECLI voltou de uma chamada;
- **é fiel?** o sumário, o tribunal, o relator e a data que afirma batem com a fonte;
- **vigência:** não afirme vigência de AUJ (ver acima).

Um número, sumário, relator ou link só entra na resposta se veio da tool. Sem retorno, é NÃO
LOCALIZADA (possível alucinação) - reporte assim, nunca "provavelmente existe".

## Como citar (obrigatório)

- **Sempre** cite o campo `link_completo` do registo devolvido - é a URL estável do acórdão na
  DGSI. **Nunca invente** número, sumário, descritor ou link.
- Cite o `tribunal`, o `numero_processo`, o `relator` e a `data_julgamento` quando presentes.
- **O sumário que recebe é um EXCERTO, não o sumário inteiro, e vem cortado a meio de palavra.**
  O campo chama-se `ementa_snippet` e traz cerca de 280 caracteres em `buscar_hibrida` (1200 em
  `buscar_semantica`). Resuma-o em 1-2 frases **sem completar a frase truncada** e sem inferir o
  que viria a seguir: para o teor integral, remeta o utilizador ao `link_completo`, que abre a
  ficha da DGSI com o **Sumário** e a **Decisão Texto Integral** completos.
- **ECLI e descritores NÃO vêm nestas tools.** Existem na base e na ficha da DGSI, mas nenhuma
  tool MCP os projecta hoje. Não os prometa, não os procure no envelope e sobretudo não os
  reconstitua a partir do número do processo - um ECLI inventado tem a forma certa e é falso.
- Se `desfecho` for `sem_resultado` (ou só hits fracos), **diga-o honestamente**.
  Se for `erro` ou `nao_terminou`, reporte a falha. Não preencha com precedente fabricado.

## Vocabulário (direito português, não brasileiro)

Use: acórdão, Tribunal da Relação, Supremo Tribunal de Justiça, acórdão uniformizador de
jurisprudência (AUJ), descritores, sumário, ECLI, força obrigatória geral. **Não** transponha
instituições, precedentes qualificados ou sistemas de classificação de outros ordenamentos.

## Tools deliberadamente FORA desta skill

Não as reponha sem medir a base primeiro. Duas continuam fora, e pelo mesmo motivo: devolvem
vazio, e um vazio delas não se lê como "sem dado" mas como uma afirmação falsa sobre o direito.

**Uma terceira saiu desta lista em 2026-07-29** - `buscar_qualificada` passou a responder. Isso é
a lição a reter: **esta lista descreve o estado da base num dia, não uma propriedade permanente
do produto.** Antes de a repetir a um utilizador, meça; e quando uma delas passar a responder,
corrija aqui em vez de continuar a dizer que não existe.

- `buscar_por_citacoes` - a rede de citações portuguesa tem **0 arestas**. Um vazio leria-se
  como "ninguém citou este acórdão". O servidor recusa-as explicitamente em vez de responder
  vazio; a skill `fora-de-ambito-pt` ensina a ler esse envelope de recusa sem o converter numa
  afirmação sobre o Direito.
- `obter_versoes_qualificada` - o histórico de versões tem **0 linhas**. Um vazio leria-se como
  "a redação nunca mudou".
- `buscar_qualificada` - **deixou de estar nesta lista: já responde.** A espécie passou a ser
  selector aceite em PT e `tipo='auj'` devolve resultados reais (medido no servidor vivo em
  2026-07-29). Continua a ser verdade que `numero` e `materia` não selecionam nada, e continua
  a ser verdade que a chamada **não enumera** o acervo - as condições exatas estão na secção
  AUJ acima, e é lá que deve ler antes de a usar. Se lhe chegar `erro de contrato`, chamou-a
  sem selector nenhum: passe `tipo`.

## Autenticação e aprovação de ferramentas

- O cliente MCP autentica por si: **login OAuth** (o navegador abre no primeiro uso) **ou**
  chave `ik_*` no cabeçalho `Authorization: Bearer` (canal privado). Um **401** indica sessão
  ou chave em falta/expirada: peça ao utilizador para refazer o login ou rever a chave; **nunca**
  cole a chave em conversa nem em commit.
- Todas as tools são **somente-leitura** (`readOnlyHint`): não escrevem nada. Se o cliente pedir
  aprovação por chamada, oriente o utilizador a autorizar uma vez e marcar "sempre permitir"; é
  seguro liberar em bloco. Prefira a pesquisa direta (`buscar_hibrida`/`buscar_semantica`), que
  já devolve sumário e `link_completo` oficial de cada acórdão.
