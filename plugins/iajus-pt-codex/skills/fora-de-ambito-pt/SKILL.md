---
name: fora-de-ambito-pt
description: 'Responde honestamente ao que o corpus português da IAJUS NÃO serve: legislação portuguesa (leis, decretos-leis, códigos), doutrina, vigência e classificação temática. Accione quando pedirem "qual a lei que regula X", "o que diz o artigo N.º do Código Civil", "esta norma ainda está em vigor", ou quando uma tool devolver uma recusa do servidor. Impede que o assistente cite direito português de memória e encaminha para o Diário da República e para a jurisprudência que aplica a norma.'
allowed-tools: mcp__iajus-pt__buscar_fts, mcp__plugin_iajus-pt_iajus-pt__buscar_fts, mcp__iajus-pt__buscar_regex, mcp__plugin_iajus-pt_iajus-pt__buscar_regex
---

# O que esta superfície não serve, e como responder mesmo assim (IAJUS)

O servidor MCP `iajus-pt` serve **jurisprudência portuguesa e mais nada**. Esta skill existe
porque a resposta errada a um pedido fora desse âmbito não é "não sei": é o assistente
**inventar direito português** para não ficar de mãos vazias - citar um artigo de memória, dar
por vigente uma norma que não consultou, transpor uma figura de outro ordenamento. Isso é uma
afirmação falsa sobre o Direito entregue a quem confia nela.

Accione esta skill quando o pedido for de legislação, de doutrina ou de vigência, e sempre que
uma tool devolver uma **recusa** do servidor (ver a leitura dos envelopes, abaixo).

## Envelope de desfecho

Leia a chave `desfecho` ANTES de qualquer contagem. Os cinco valores são mutuamente exclusivos:

- `erro` — a consulta FALHOU; ninguém consultou o acervo. Não é ausência.
- `sem_resultado` — a consulta CORREU e o acervo não tem. Zero MEDIDO.
- `nao_terminou` — tempo esgotado ou tecto. NÃO-MEDIDO; não afirme que «não existe».
- `parcial` — mediu uma parte; declare o que ficou de fora.
- `medida_indisponivel` - a fonte respondeu e NÃO carrega a medida. NÃO-MEDIDO sem avaria; não é zero.

`total: 0` só é ausência medida quando `desfecho` é `sem_resultado`. Sem `desfecho`, ou com `erro`/`nao_terminou`/`medida_indisponivel`, a superfície não mediu — nunca «não existe legislação».

## A regra que manda em tudo o resto

**Nunca cite, transcreva nem parafraseie o texto de uma norma portuguesa que não veio de uma
fonte consultada nesta sessão.** Não há nesta superfície nenhuma tool que devolva o texto de uma
lei portuguesa - portanto, se aparecer um artigo na resposta, ele veio da memória do modelo, e a
memória do modelo não é fonte de direito. Dizer "o artigo 483.º estabelece que..." sem o ter
lido é o defeito que esta skill existe para impedir, mesmo quando a formulação parece certa.

## O que está fora, e porquê

| Pedido | Estado nesta superfície | O que fazer |
|---|---|---|
| Texto de uma lei, decreto-lei, código ou artigo português | **Não servido.** Não existe tool de legislação neste perfil, e o servidor exclui a família `legislacao` também na camada de chamada, para que uma pesquisa de texto não devolva norma por engano. | Diga que esta via não serve legislação e remeta ao **Diário da República** (`diariodarepublica.pt`), que é a fonte oficial. Depois ofereça o que ESTA base tem: a jurisprudência que aplica a norma (ver abaixo). |
| "Esta norma ainda está em vigor?" | **Não medido.** A base não serve normas nem regista a sua vigência. | Não responda por dedução nem por memória. Remeta à fonte oficial. |
| Doutrina, manuais, anotações | **Não servido.** | Diga-o e não o anuncie como "em breve". |
| Classificação temática, ontologia jurídica, taxonomia de assuntos | **Não servido.** | Use descritores e termos do próprio acórdão, que a pesquisa devolve. |
| Legislação regional ou autárquica | **Não servido**, e note-se que Portugal é um Estado unitário: não existe aqui a estrutura de legislação por unidade federada que outros ordenamentos têm, pelo que não há sequer uma superfície equivalente a prometer. | Remeta ao Diário da República e, para o âmbito regional, às publicações oficiais das regiões autónomas. |

## O que ainda se consegue entregar (e é muito)

Um pedido de legislação quase nunca é só um pedido de texto: por trás está uma questão jurídica.
A base responde à parte que lhe compete - **como os tribunais portugueses aplicam aquela norma**.

1. Peça ao utilizador a norma e o artigo em causa.
2. Procure a **forma literal da citação** com `buscar_regex` (por exemplo, o artigo tal como se
   escreve num acórdão) ou a expressão técnica com `buscar_fts`. Inclua um trecho literal com
   pelo menos 3 caracteres; `buscar_regex` recusa padrões só de metacaracteres.
3. Entregue os acórdãos que interpretam ou aplicam o preceito, com o `link_completo` da DGSI - e
   deixe claro que está a citar **jurisprudência sobre a norma**, não a norma.
4. Se a questão for de pesquisa jurisprudencial a sério (escalada de modalidade, janelas por
   tribunal, acórdãos uniformizadores), passe para a skill `pesquisar-jurisprudencia-pt`, que é
   quem tem o método completo.

Estas duas tools varrem apenas jurisprudência: o servidor filtra a família `legislacao` do
resultado. Um resultado sem normas **não significa que a norma não exista** - significa que esta
superfície não a serve.

## Ler as recusas do servidor sem as converter em mentira

O servidor recusa em vez de responder vazio, de propósito: um vazio silencioso lê-se como uma
afirmação sobre o Direito. Há dois envelopes, e cada um diz uma coisa diferente.

**1. Ferramenta fora de alcance neste acervo.** Reconhece-se por
`"error_code": "tool_unavailable_on_profile"`, com `"disponivel": false` e, decisivo,
`"acervo_consultado": false`. Repare que o envelope **não traz `resultados` nem `total`** - é
propositado, para que ninguém leia zero onde não houve consulta nenhuma.

> Leitura correta: *a superfície não alcança esta via*. Leitura FALSA e proibida: "não há
> resultados", "não existe". O envelope traz o `motivo` medido; use-o para explicar ao
> utilizador e prossiga pelas restantes ferramentas de pesquisa. `retriable` é `false`: repetir
> a chamada não muda nada.

Duas tools do perfil respondem assim, e cada uma induziria uma falsidade diferente:

- `buscar_por_citacoes` - a rede de citações portuguesa tem **0 arestas**. Um vazio leria-se como
  "ninguém citou este acórdão".
- `obter_versoes_qualificada` - o histórico de versões tem **0 linhas**. Um vazio leria-se como
  "a redação nunca mudou".

**2. Família fora do âmbito do perfil.** Reconhece-se por `familia_recusada` e
`familias_fora_de_escopo` no envelope (e, num resultado filtrado,
`familias_fora_de_escopo_removidas`). É o que acontece a um pedido de legislação através de uma
tool de pesquisa.

> Leitura correta: *esta superfície serve jurisprudência; a família pedida está fora*. Leitura
> FALSA e proibida: "não existe legislação sobre isto".

Em qualquer dos dois casos: **diga ao utilizador o que aconteceu, em vez de apresentar a recusa
como um resultado.** E nunca a compense com conhecimento próprio.

## Vocabulário e ordenamento

Escreva em português europeu e mantenha-se no direito português: acórdão, Tribunal da Relação,
Supremo Tribunal de Justiça, acórdão uniformizador de jurisprudência, descritores, sumário,
ECLI, Diário da República. **Não** transponha instituições, categorias de precedente nem
sistemas de classificação de outros ordenamentos - o utilizador leria isso como direito
português, e seria falso.

## Autenticação

O cliente MCP autentica por si: **login OAuth** (o navegador abre no primeiro uso) **ou** chave
`ik_*` no cabeçalho `Authorization: Bearer` (canal privado). Um **401** indica sessão ou chave em
falta ou expirada, e **não** é uma recusa de âmbito: peça ao utilizador para refazer o login ou
rever a chave; **nunca** cole a chave em conversa nem em commit. Todas as tools são
somente-leitura.
