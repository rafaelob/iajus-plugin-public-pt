---
name: estado-corpus-pt
description: Mostra o que o corpus português da IAJUS contém AGORA (por tribunal, ano e acervo) pelo MCP IAJUS. Acione antes de concluir que "não existe" jurisprudência para um tribunal/ano - um resultado vazio na pesquisa pode ser cobertura em andamento, e esta skill confirma o que já está na base.
allowed-tools: mcp__iajus-pt__obter_estatisticas_base, mcp__plugin_iajus-pt_iajus-pt__obter_estatisticas_base
---

# Estado do corpus português (IAJUS)

Tem acesso à tool `obter_estatisticas_base`, que devolve o estado atual do corpus PT. **Use-a
para confirmar o retrato atual antes de afirmar cobertura ou ausência.**

Enumere apenas os acervos que a resposta trouxer. Um acervo que não aparece **não está nesta
superfície** - não o anuncie, não o descreva como "em breve" e não infira a sua dimensão.

## Estado da tool nesta superfície (medido 2026-07-28)

A tool está a servir o corpus português com um defeito conhecido, já reportado e em correção.
**Leia isto antes de a chamar**, para não interpretar o erro como ausência de dados:

- As secções por **família** e por **tribunal** (`secao="tudo"`, `"familias"`, `"orgaos"`)
  devolvem hoje **apenas um envelope de erro** no corpus português: dependem de um agregado que
  ainda não existe neste banco. **O erro é defeito da nossa superfície, não ausência de acervo** -
  a jurisprudência portuguesa está lá, completa e indexada. Nunca a reporte como vazia por isto.
- A secção `secao="qualificadas"` **responde** e é a única fiável hoje.
- Enquanto o defeito durar, use a tabela de cobertura desta skill (abaixo) para responder a
  perguntas de cobertura, e diga que é um retrato medido em 2026-07-28, não uma leitura ao vivo.

> **Ao ler `secao="qualificadas"`, corrija a contagem — mas confira primeiro o total.** Em
> 2026-07-28 a tool devolvia **682** acórdãos na espécie `acordao_uniformizador_jurisprudencia`,
> dos quais **33** eram de Tribunais da Relação, que **não podem uniformizar jurisprudência**
> (art. 686.º CPC): são acórdãos que citam um AUJ, não AUJ. Dá **649**, todos do Supremo.
>
> ⚠️ **Esse 649 é derivado daquele 682, e não se recalcula sozinho.** A tool não reparte por
> tribunal, portanto a subtração não é verificável a partir do que ela devolve. Regra:
> **se `quantidade` vier 682, comunique 649**; **se vier qualquer outro número, o 649 caducou** -
> diga o total que a tool devolveu, diga que a repartição por tribunal **não está medida** nesta
> versão, e **não invente** a subtração. Um número derivado que sobrevive à mudança do número de
> onde saiu deixa de ser medida e passa a ser folclore.
>
> Os campos `vigentes` e `canceladas` vêm a zero porque a base não regista vigência de AUJ - isso
> significa **não medido**, nunca "nenhum está vigente".

## Cobertura por tribunal (medida 2026-07-28)

331.045 acórdãos de 12 tribunais portugueses. Retrato do momento da medição - o corpus é vivo e
cresce entre sessões.

| Tribunal | Anos na base |
|---|---|
| Supremo Tribunal de Justiça | 1994-2026 |
| Supremo Tribunal Administrativo | 1950-2026 |
| Tribunal da Relação de Lisboa | 1992-2026 |
| Tribunal da Relação do Porto | 1994-2026 |
| Tribunal da Relação de Coimbra | 1965-2026 |
| Tribunal da Relação de Évora | 1998-2026 |
| Tribunal da Relação de Guimarães | 2002-2026 |
| Tribunal Central Administrativo Sul | 1997-2026 |
| Tribunal Central Administrativo Norte | 2004-2026 |
| Tribunal Constitucional | **1983-1998** |
| Conflitos | 1969-2026 |
| Tribunal de Contas | 2001-2026 |

> **Tribunal Constitucional: a base termina em 1998.** Esta é uma **fronteira**, não cobertura em
> andamento: não há acórdão do TC posterior a 1998 na base. Para constitucionalidade recente,
> diga-o e remeta o utilizador a www.tribunalconstitucional.pt.

**A base é de jurisprudência.** Não há nesta superfície legislação pesquisável por sentido, nem
doutrina, nem ontologia, nem classificação temática. Se a resposta da tool não trouxer um acervo,
ele não existe aqui - não o anuncie como "em breve". Para responder a um pedido de legislação, de
doutrina ou de vigência sem inventar direito português, use a skill `fora-de-ambito-pt`.

## Quando usar

- Antes de reportar que "não há" jurisprudência de um tribunal/ano. Leia `desfecho`:
  `sem_resultado` confirma que a consulta correu e não devolveu registos;
  `erro`/`nao_terminou`/`medida_indisponivel` significa que a consulta não mediu.
- Para dar ao utilizador um panorama honesto: enumere somente os tribunais, anos e acervos
  presentes na resposta atual (ou na tabela acima, enquanto a tool estiver degradada).
- Para confirmar a janela temporal disponível de um tribunal antes de uma pesquisa por faixa de
  ano - com atenção especial ao Tribunal Constitucional.

## Envelope de desfecho

Leia a chave `desfecho` ANTES de qualquer contagem. Os cinco valores são mutuamente exclusivos:

- `erro` — a consulta FALHOU; ninguém consultou o acervo. Não é ausência.
- `sem_resultado` — a consulta CORREU e o acervo não tem. Zero MEDIDO.
- `nao_terminou` — tempo esgotado ou tecto. NÃO-MEDIDO; não afirme que «não existe».
- `parcial` — mediu uma parte; declare o que ficou de fora.
- `medida_indisponivel` - a fonte respondeu e NÃO carrega a medida. NÃO-MEDIDO sem avaria; não é zero.

`total: 0` só é ausência medida quando `desfecho` é `sem_resultado`. Sem `desfecho`, ou com `erro`/`nao_terminou`/`medida_indisponivel`, diga que a consulta não mediu.

## Como interpretar

- O corpus é **vivo**: os números crescem entre sessões à medida que novos anos são ingeridos.
  Reporte o estado como um retrato do momento, não como um total final.
- Se um tribunal esperado aparecer com contagem baixa, zero ou estiver ausente, reporte exatamente
  esse estado. Não conclua se é ausência na fonte, ingestão pendente ou cobertura incompleta sem
  um campo autoritativo. Cite o timestamp ou frescor quando a resposta o trouxer; caso contrário,
  diga que é o estado no momento da consulta.
- **Zero num campo de vigência é "não medido"**, não "nenhum vigente".

## Honestidade

Reporte os números tal como a tool os devolve, com a única correção explicitamente indicada
acima (682 -> 649). Não arredonde para cima, não repita totais estáticos como se fossem leitura
ao vivo e não afirme completude que a tool não confirma. Se a tool devolver um envelope de erro,
**diga que é um defeito conhecido da nossa superfície** e responda pela tabela desta skill - nunca
invente contagens e nunca converta o erro em "não há acervo".

## Autenticação

OAuth 2.1 é o caminho canónico. Uma chave `ik_*` no cabeçalho `Authorization: Bearer` é apenas o
fallback documentado. Um **401** indica sessão/chave em falta ou expirada. A tool é somente-leitura.
