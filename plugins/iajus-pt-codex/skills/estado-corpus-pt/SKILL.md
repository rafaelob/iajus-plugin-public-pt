---
name: estado-corpus-pt
description: Mostra o que o corpus português da IAJUS contém AGORA (por tribunal, ano e acervo) pelo MCP IAJUS. Acione antes de concluir que "não existe" jurisprudência para um tribunal/ano - um resultado vazio na pesquisa pode ser cobertura em andamento, e esta skill confirma o que já está na base.
allowed-tools: mcp__iajus-pt__obter_estatisticas_base, mcp__plugin_iajus-pt_iajus-pt__obter_estatisticas_base
---

# Estado do corpus português (IAJUS)

Tem acesso à tool `obter_estatisticas_base`, que devolve o estado atual do corpus PT. **Chame-a
antes de afirmar cobertura ou ausência.** Os únicos números que se reportam são os que a tool
devolver nesta chamada. Não há censo nesta skill; não se preenche a partir da memória.

Enumere apenas os acervos que a resposta trouxer. Um acervo que não aparece **não está nesta
superfície** - não o anuncie, não o descreva como "em breve" e não infira a sua dimensão.

## Secções da tool

- As secções `familias`, `orgaos` e `tudo` podem devolver um **envelope de erro**. Isso é
  defeito da superfície, **não é ausência** de jurisprudência portuguesa. Não invente
  contagens. Não preencha a partir da memória nem de qualquer censo gravado nesta skill.
  Diga que a consulta **não mediu** essa secção. **Não converta o erro** em «não há acervo».
- A secção `qualificadas` **pode responder** quando as outras devolvem envelope de erro; nesse
  caso é a **única secção** com números. Reporte o que cada secção devolveu de facto - não
  trate a que respondeu como prova das que falharam.

## Envelope de desfecho

Leia a chave `desfecho` ANTES de qualquer contagem. Os cinco valores são mutuamente exclusivos:

- `erro` — a consulta FALHOU; ninguém consultou o acervo. Não é ausência.
- `sem_resultado` — a consulta CORREU e o acervo não tem. Zero MEDIDO.
- `nao_terminou` — tempo esgotado ou tecto. NÃO-MEDIDO; não afirme que «não existe».
- `parcial` — mediu uma parte; declare o que ficou de fora.
- `medida_indisponivel` - a fonte respondeu e NÃO carrega a medida. NÃO-MEDIDO sem avaria; não é zero.

`total: 0` só é ausência medida quando `desfecho` é `sem_resultado`. Sem `desfecho`, ou com
`erro`/`nao_terminou`/`medida_indisponivel`, diga que a consulta não mediu.

## Quando usar

- Antes de reportar que «não há» jurisprudência de um tribunal/ano. Leia `desfecho` primeiro.
- Para um panorama honesto: enumere somente os tribunais, anos e acervos presentes na resposta
  atual da tool.
- Para confirmar a janela temporal de um tribunal, use só os anos que a tool devolver. Um ano
  em falta nesta resposta não está medido nesta chamada.

## Como interpretar

- O corpus é **vivo**: os números mudam entre sessões. Reporte o estado como retrato do momento
  da consulta, não como total final.
- Se um tribunal esperado aparecer com contagem baixa, zero ou estiver ausente, reporte
  exatamente esse estado. Não conclua se é ausência na fonte, ingestão pendente ou cobertura
  incompleta sem um campo autoritativo. Cite o timestamp ou frescor quando a resposta o
  trouxer; caso contrário, diga que é o estado no momento da consulta.
- **Zero num campo de vigência é «não medido»**, nunca «nenhum vigente».

## Acórdãos uniformizadores

Se a tool devolver contagens de acórdão uniformizador de jurisprudência, reporte o número que
a tool devolveu. **Não subtraia 33** nem aplique qualquer correção derivada. Se a tool expor
`uniformizador_valido` ou uma repartição por tribunal, reporte-a; se não expor, diga que a
repartição **não está medida**.

## Âmbito

**A base, nesta superfície, é de jurisprudência.** Não há legislação pesquisável por sentido,
nem doutrina, nem ontologia, nem classificação temática. Se a resposta da tool não trouxer um
acervo, ele não está aqui - não o anuncie como «em breve». Para legislação, doutrina ou
vigência sem inventar direito português, use a skill `fora-de-ambito-pt`.

## Honestidade

Reporte os números tal como a tool os devolve. Não arredonde para cima, não repita totais
estáticos como se fossem leitura ao vivo e não afirme completude que a tool não confirma.
Se a tool devolver um envelope de erro, **não invente contagens** e **não converta o erro**
em «não há acervo».

## Autenticação

OAuth 2.1 é o caminho canónico. Uma chave `ik_*` no cabeçalho `Authorization: Bearer` é apenas o
fallback documentado. Um **401** indica sessão/chave em falta ou expirada. A tool é somente-leitura.
