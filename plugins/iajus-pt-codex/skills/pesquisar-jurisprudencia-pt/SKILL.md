---
name: pesquisar-jurisprudencia-pt
description: "Pesquisa jurisprudência portuguesa com modalidade, filtros e proveniência compatíveis com as ferramentas expostas nesta ligação."
allowed-tools: mcp__iajus-pt__pesquisar_decisoes, mcp__plugin_iajus-pt_iajus-pt__pesquisar_decisoes, mcp__iajus-pt__pesquisar_precedentes, mcp__plugin_iajus-pt_iajus-pt__pesquisar_precedentes, mcp__iajus-pt__pesquisa_juridica_lex, mcp__plugin_iajus-pt_iajus-pt__pesquisa_juridica_lex, mcp__iajus-pt__buscar_semantica, mcp__plugin_iajus-pt_iajus-pt__buscar_semantica, mcp__iajus-pt__buscar_hibrida, mcp__plugin_iajus-pt_iajus-pt__buscar_hibrida, mcp__iajus-pt__buscar_fts, mcp__plugin_iajus-pt_iajus-pt__buscar_fts, mcp__iajus-pt__buscar_regex, mcp__plugin_iajus-pt_iajus-pt__buscar_regex, mcp__iajus-pt__buscar_qualificada, mcp__plugin_iajus-pt_iajus-pt__buscar_qualificada
---

# Pesquisar jurisprudência portuguesa (IAJUS)

Veja primeiro as ferramentas expostas nesta ligação. Use apenas as que aparecem e siga o esquema de cada uma. Se pesquisar_decisoes estiver disponível, envie busca.modo com um dos modos PT abaixo. Se a ligação expuser apenas ferramentas legadas, use o nome e os argumentos próprios dessa ferramenta; não envie o objeto busca a uma ferramenta legada.

## Escolher a modalidade

| Intenção | Modo de pesquisar_decisoes | Interface legada, apenas se exposta |
|---|---|---|
| Questão por significado e termos complementares | hibrida | buscar_hibrida |
| Tema ou conceito, sem exigir correspondência literal | semantica | buscar_semantica |
| Termos presentes no texto, ou frase exata | textual | buscar_fts |
| Padrão literal ou forma de citação | expressao | buscar_regex |

A entrada de pesquisar_decisoes é fechada e varia por modo. Use apenas filtros que o modo aceita:

- hibrida: tribunal, classe, tipo_recurso e os campos comuns de ano, data, relator, orgao_julgador, tipo e tipo_unidade;
- semantica: tribunal, um ano e kind;
- textual: tribunal, classe_sigla, frase_exata e os campos comuns;
- expressao: tribunal, classe_sigla, ignorar_maiusculas, allow_unindexed_scan e os campos comuns.

Os campos comuns são ano_min, ano_max, data_julgamento_min, data_julgamento_max, relator, orgao_julgador, tipo e tipo_unidade. limite vai de 1 a 100 e, por defeito, é 20; ajuste à quantidade pedida. Não acrescente filtros de outro modo ou do Brasil.

Para obter mais resultados, repita consulta, filtros e tamanho com page_info.next_cursor em busca.cursor. Pare na quantidade pedida ou quando não houver continuação; não percorra todas as páginas sem necessidade. Híbrida, semântica e textual percorrem uma janela de até três páginas, limitada a 100 resultados (60 com páginas de 20); expressão usa a paginação do executor e pode não ter cursor na pesquisa sem índice. Nomeadamente, a localização textual por número exato mantém as restrições próprias. Fim da janela ou truncated não prova fim do acervo: refine o recorte. Cursor expirado, inválido ou ranking alterado exige reiniciar, sem contar a primeira página repetida como novos resultados.

Em expressao, use uma expressão regular POSIX com âncora literal; só permita allow_unindexed_scan quando o pedido justificar uma pesquisa sem essa âncora. ignorar_maiusculas não elimina diferenças de acentuação.

## Precedentes

Se pesquisar_precedentes estiver exposta, a entrada busca exige filtros.tipo. Para acórdãos uniformizadores, use tipo auj; tribunal é opcional e incluir_canceladas é verdadeiro por defeito. limite aceita até 10 resultados e não há cursor. Preserve estados e avisos devolvidos; desconhecido continua desconhecido. Um resultado curto ou zero não é um inventário do acervo.

Se só estiver exposta buscar_qualificada, use o seu esquema anunciado. Não lhe envie os filtros de pesquisar_precedentes.

Leia `uniformizador_valido` em cada resultado: se for falso, não apresente o acórdão como uniformizador, mesmo que o tipo devolvido seja `auj`. Preserve a indicação de classificação inconsistente; um acórdão de Relação não deve ser promovido a AUJ. Vigência desconhecida não significa vigente.

## Resultado e citação

Cite apenas elementos devolvidos nesta chamada: tribunal, número, data, excerto e ligação de origem quando presentes. Preserve a proveniência e não rotule uma ligação como oficial sem indicação da própria fonte. Um excerto não sustenta afirmações que não contém.

Uma pesquisa concluída sem resultados é zero para esse pedido e recorte. Erro, timeout, interrupção, resposta parcial ou medida indisponível não são ausência. Não trate o limite de resultados como censo nem afirme completude.

Esta pesquisa não acrescenta leitor de documentos, legislação, doutrina, histórico de decisões ou grafo de relações. Para uma pergunta jurídica contextual mais ampla, use a skill pesquisa-juridica-lex-pt apenas se a ferramenta correspondente estiver exposta.
