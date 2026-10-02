---
name: verificar-citacoes-pt
description: "Confere citações de jurisprudência portuguesa uma a uma contra os registos e fontes devolvidos nesta ligação."
allowed-tools: mcp__iajus-pt__pesquisar_decisoes, mcp__plugin_iajus-pt_iajus-pt__pesquisar_decisoes, mcp__iajus-pt__pesquisar_precedentes, mcp__plugin_iajus-pt_iajus-pt__pesquisar_precedentes, mcp__iajus-pt__buscar_semantica, mcp__plugin_iajus-pt_iajus-pt__buscar_semantica, mcp__iajus-pt__buscar_hibrida, mcp__plugin_iajus-pt_iajus-pt__buscar_hibrida, mcp__iajus-pt__buscar_fts, mcp__plugin_iajus-pt_iajus-pt__buscar_fts, mcp__iajus-pt__buscar_regex, mcp__plugin_iajus-pt_iajus-pt__buscar_regex, mcp__iajus-pt__buscar_qualificada, mcp__plugin_iajus-pt_iajus-pt__buscar_qualificada
---

# Verificar citações de jurisprudência portuguesa (IAJUS)

Extraia cada citação e a afirmação que o texto lhe atribui. Consulte a lista de ferramentas desta ligação e use apenas as ferramentas e esquemas nela presentes. Para pesquisa de raiz, use a skill pesquisar-jurisprudencia-pt.

## Pesquisar cada citação

| Referência | Pesquisa adequada |
|---|---|
| Número de processo, ECLI ou identificador literal | expressao em pesquisar_decisoes, ou buscar_regex se essa interface legada estiver exposta |
| Trecho citado entre aspas | textual com frase exata em pesquisar_decisoes, ou buscar_fts se estiver exposta |
| Tese sem número | semantica e depois hibrida, ou as ferramentas legadas equivalentes que estiverem expostas |
| Acórdão uniformizador | pesquisar_precedentes com filtros.tipo auj, ou buscar_qualificada apenas se estiver exposta |

Com pesquisar_decisoes, construa busca segundo o modo e os filtros PT aceites. Não envie essa estrutura a uma ferramenta legada; siga o esquema legado que a própria ligação apresenta.

## Veredictos

- CONFIRMADA: encontrou o registo e os campos devolvidos sustentam a referência e a afirmação.
- DIVERGENTE: encontrou o registo, mas um dado devolvido contradiz a afirmação; indique precisamente a diferença.
- NÃO LOCALIZADA: uma pesquisa concluída não encontrou a referência. Significa apenas que não foi localizada neste pedido e recorte, não que a citação seja necessariamente fabricada.
- NÃO VERIFICÁVEL: erro, timeout, resposta parcial, medida indisponível ou excerto insuficiente para comparar a afirmação.
- FORA DE ÂMBITO: legislação, doutrina ou jurisprudência de outro ordenamento.

Nunca confirme de memória. Não infira vigência, força ou relações entre acórdãos a partir da presença de um registo. Se o serviço devolver uma ligação de origem, preserve-a e descreva a origem como indicada; não invente links nem apresente automaticamente a fonte como oficial.

No final, conte cada veredicto separadamente e destaque as citações não localizadas ou não verificáveis.
