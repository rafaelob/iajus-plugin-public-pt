---
name: verificar-citacoes-pt
description: "Confere citações de jurisprudência portuguesa uma a uma contra os registos e fontes devolvidos nesta ligação."
---

# Verificar citações de jurisprudência portuguesa (IAJUS)

No Codex, se as ferramentas IAJUS não estiverem listadas ou toda chamada voltar 401 ou não autorizado, peça ao utilizador que execute `codex mcp login iajus-pt` e reinicie a sessão; entretanto, não responda de memória.

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

Reaja aos sinais da pesquisa como em pesquisar-jurisprudencia-pt: input_contract_refused, corrija a entrada; filtro_nao_suportado ou filtros_ignorados, retire o filtro nomeado e informe que não foi aplicado; parcial com janela_limitada, continue pelo cursor; incompleta com falha_busca ou timeout, repita uma vez mais estreito e, persistindo, declare NÃO VERIFICÁVEL, nunca NÃO LOCALIZADA. Um acórdão que apenas cita o número não é o processo citado.

Os dados de julgamento (relator, vencidos, votos, resultado, tese, data) só valem se vierem de um excerto devolvido nesta conversa; sem ele, escreva «não localizei esse dado no excerto do acórdão». Se o utilizador contestar o veredicto, volte a consultar pelo número completo e cite o excerto.

Nunca confirme de memória. Não infira vigência, força ou relações entre acórdãos a partir da presença de um registo. Se o serviço devolver uma ligação de origem, preserve-a e descreva a origem como indicada; não invente links nem apresente automaticamente a fonte como oficial.

No final, conte cada veredicto separadamente e destaque as citações não localizadas ou não verificáveis.
