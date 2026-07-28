---
name: pesquisar-jurisprudencia-pt
description: Pesquisa e cita jurisprudência portuguesa real (Supremo Tribunal de Justiça, Tribunais da Relação, STA, TCA-Sul/Norte, Tribunal Constitucional, Conflitos, Tribunal de Contas) pelo MCP IAJUS - modalidades semântica, híbrida, texto integral e regex. Acione para acórdão, descritor, sumário, ECLI, tese firmada ou entendimento de um tribunal português.
allowed-tools: mcp__iajus-pt__buscar_semantica, mcp__plugin_iajus-pt_iajus-pt__buscar_semantica, mcp__iajus-pt__buscar_hibrida, mcp__plugin_iajus-pt_iajus-pt__buscar_hibrida, mcp__iajus-pt__buscar_fts, mcp__plugin_iajus-pt_iajus-pt__buscar_fts, mcp__iajus-pt__buscar_regex, mcp__plugin_iajus-pt_iajus-pt__buscar_regex
---

# Pesquisar jurisprudência portuguesa (IAJUS)

Tem acesso ao servidor MCP `iajus-pt`, que indexa jurisprudência portuguesa dos tribunais
superiores e das Relações, recolhida da fonte oficial DGSI (www.dgsi.pt), mais o Tribunal de
Contas. **Use o MCP em vez de inventar jurisprudência - a fonte é a verdade; nunca cite de
memória.**

> **Corpus VIVO e em crescimento:** a base é ingerida continuamente; órgãos e anos novos
> aparecem na pesquisa automaticamente, sem alteração de skill. Um `total: 0` para um órgão/ano
> em cobertura significa **cobertura em andamento**, não "não existe": avise o utilizador e
> ofereça uma alternativa (por exemplo, o tribunal superior). Uma excepção real está abaixo, na
> janela do Tribunal Constitucional - essa é fronteira, não atraso.

## Órgãos cobertos (DGSI + Tribunal de Contas)

Janelas medidas em 2026-07-28. A janela serve para **interpretar o vazio**; o slug serve para
filtrar (`orgao_code`), sempre na forma `pt_<x>`, que é inequívoca - ver as notas de uso.

| Tribunal | Slug | Anos na base | Nota |
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
| Tribunal Constitucional | `pt_tc` | **1983-1998** | Fiscalização da constitucionalidade (Art. 281.º CRP). **Ver o aviso abaixo.** |
| Conflitos | `pt_conflitos` | 1969-2026 | Conflitos de jurisdição/competência. **Pode repetir resultados**. |
| Tribunal de Contas | `pt_tdc` | 2001-2026 | Fiscalização prévia e concomitante das contas públicas. |

> **Tribunal Constitucional: a base termina em 1998.** Não há na base acórdão do TC posterior a
> 1998. Para uma questão de constitucionalidade recente, **diga-o explicitamente** e não o
> apresente como "cobertura em andamento" nem escale para o TC à espera de resultado: não vem.
> Ofereça o que existe - a apreciação da constitucionalidade que o STJ, o STA ou as Relações
> fazem nos seus próprios acórdãos - e remeta o utilizador ao sítio oficial
> (www.tribunalconstitucional.pt) para a jurisprudência constitucional posterior a 1998.

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
  eleger o Brasil), **mas a correcção só vale depois de o servidor ser actualizado**. Escreva o
  slug e a questão não se põe.
- **O slug alcança o tribunal inteiro; o que a grafia dupla estraga é a repetição.** Cinco
  órgãos estão guardados sob duas grafias (`pt_<x>` e `<x>_pt`), mas medido em 2026-07-28 o
  filtro `pt_<x>` alcança **todos** os acórdãos distintos de cada um deles. O efeito real é
  outro: **o mesmo acórdão pode voltar mais de uma vez** na mesma lista de resultados. Antes de
  apresentar, deduplique pelo `link_completo` e não confunda duas ocorrências com dois
  precedentes. Defeito conhecido, em correcção no servidor.
- **Recorte por ano:** as modalidades aceitam ano ou faixa (`ano_min`/`ano_max`). **Confirme
  sempre a data do próprio resultado** antes de a citar, em vez de assumir que o filtro a
  garantiu.
- `buscar_regex` recusa padrões só de metacaracteres; inclua um trecho literal com pelo menos
  3 caracteres.

## Acórdãos uniformizadores de jurisprudência (AUJ)

**Existem 649 AUJ do Supremo Tribunal de Justiça na base** e citam-se pelo teor. Só o Supremo
uniformiza jurisprudência (art. 686.º CPC): um acórdão de Relação nunca é AUJ, por muito que
cite um.

**Como chegar lá:** um AUJ é alcançável como qualquer outro acórdão, pelas modalidades acima -
está indexado com o mesmo texto e o mesmo `link_completo`. Pesquise a tese ("uniformização de
jurisprudência" mais o tema, ou só o tema) e leia o que volta.

**O que NÃO existe hoje - e não o encene:**

- Não há tool que **liste** ou **filtre** os AUJ. Não anuncie "vou consultar os acórdãos
  uniformizadores": pesquise o tema e identifique-os no que voltar.
- Uma pesquisa por "uniformização" **não devolve o conjunto**: só cerca de um terço dos AUJ usa
  essa palavra no sumário. Não conclua a partir do que voltou que os restantes não existem.
- A base **não registra vigência** de AUJ. **Nunca** afirme que um está vigente, revogado ou
  superado; cite pelo teor e remeta a confirmação à fonte oficial (DGSI).

## Método do pesquisador (4 passos)

Uma pesquisa jurídica a sério quase nunca é UMA chamada: é uma varredura que escala de
modalidade até a cobertura estabilizar.

1. **Escale a modalidade em vez de cair no vazio.** Uma pesquisa fraca (`total: 0`, ou hits
   cujo trecho não responde) NÃO é sinal de parar - é sinal de escalar, nesta ordem:
   - `buscar_semantica` -> **`buscar_hibrida`** com a MESMA consulta (a fusão resgata o que a
     densa isolada perdeu);
   - **reformule** com os termos que apareceram nos primeiros hits (relator, descritor,
     dispositivo citado, número de acórdão);
   - **troque de modalidade pela forma da pergunta:** termo técnico literal -> `buscar_fts`;
     citação de artigo/ECLI -> `buscar_regex`;
   - **suba na hierarquia de autoridade:** se uma Relação vier vazia para a tese, tente o
     Supremo competente (STJ para o comum, STA para o administrativo/fiscal) - a tese firmada
     costuma estar lá. **Para constitucionalidade, releia a janela do TC acima antes de subir.**
2. **Prefira o consolidado ao acórdão isolado.** Para "qual o entendimento atual", um AUJ do
   Supremo vale mais que um acórdão de Relação; se um AUJ aparecer nos resultados, ancore nele
   e use os acórdãos comuns para exemplificar a aplicação da tese.
3. **Refine até a cobertura estabilizar** (rondas sem resultado novo relevante) e cruze as
   modalidades: densa/híbrida para o panorama, FTS/regex para termos e citações literais.
4. **Envelope de honestidade.** Reporte o vazio como vazio, com o motivo: um `total: 0` de
   órgão/ano em cobertura é **cobertura em andamento**, não "o precedente não existe" (diga-o e
   ofereça a fonte superior); no TC pós-1998 é **fronteira da base**; um `{ "erro": ... }` pede
   ajuste do argumento. Nunca preencha a lacuna com um precedente plausível porém fabricado.

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
- Cite o `tribunal`, o `numero_processo`, o `relator` e a `data` (julgamento/publicação) quando
  presentes; quando houver, apresente o **ECLI** e os **descritores**. Resuma o `sumário` em 1-2
  frases.
- Se a pesquisa **não** devolver resultado relevante (`total: 0` ou hits fracos), **diga-o
  honestamente** - não preencha a lacuna com um precedente fabricado.

## Vocabulário (direito português, não brasileiro)

Use: acórdão, Tribunal da Relação, Supremo Tribunal de Justiça, acórdão uniformizador de
jurisprudência (AUJ), descritores, sumário, ECLI, força obrigatória geral. **Não** transponha
instituições, precedentes qualificados ou sistemas de classificação de outros ordenamentos.

## Tools deliberadamente FORA desta skill

Não as reponha sem medir a base primeiro. Em 2026-07-28 devolviam vazio sempre, e um vazio delas
não se lê como "sem dado" mas como uma afirmação falsa sobre o direito:

- `buscar_por_citacoes` - a rede de citações portuguesa tem **0 arestas**. Um vazio leria-se
  como "ninguém citou este acórdão".
- `obter_versoes_qualificada` - o histórico de versões tem **0 linhas**. Um vazio leria-se como
  "a redacção nunca mudou".
- `buscar_qualificada` - não tem porta de entrada em PT: `numero` guarda o identificador
  documental da DGSI (não um número de acórdão citável), `materia` depende de um campo que está
  a NULL nos 682 registos, e o tipo português (`acordao_uniformizador_jurisprudencia`) não é um
  dos valores que a tool aceita. Use as modalidades de pesquisa (secção AUJ acima).

## Autenticação e aprovação de ferramentas

- O cliente MCP autentica por si: **login OAuth** (o navegador abre no primeiro uso) **ou**
  chave `ik_*` no cabeçalho `Authorization: Bearer` (canal privado). Um **401** indica sessão
  ou chave em falta/expirada: peça ao utilizador para refazer o login ou rever a chave; **nunca**
  cole a chave em conversa nem em commit.
- Todas as tools são **somente-leitura** (`readOnlyHint`): não escrevem nada. Se o cliente pedir
  aprovação por chamada, oriente o utilizador a autorizar uma vez e marcar "sempre permitir"; é
  seguro liberar em bloco. Prefira a pesquisa direta (`buscar_hibrida`/`buscar_semantica`), que
  já devolve sumário e `link_completo` oficial de cada acórdão.
