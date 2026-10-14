---
name: verificar-citacoes-pt
description: "Confere citações de jurisprudência portuguesa uma a uma contra os registos e fontes devolvidos por pesquisar_decisoes e pesquisar_precedentes."
---

# Verificar citações de jurisprudência portuguesa (IAJUS)

No Codex, se as ferramentas IAJUS não estiverem listadas ou toda chamada voltar 401 ou não autorizado, peça ao utilizador que execute `codex mcp login iajus-pt` e reinicie a sessão; entretanto, não responda de memória.

Extraia cada citação e a afirmação que o texto lhe atribui. Consulte a lista de ferramentas desta ligação e use apenas as presentes, com o respetivo esquema. Para pesquisa de raiz, use a skill pesquisar-jurisprudencia-pt.

## Pesquisar cada citação

| Referência | Pesquisa adequada (pesquisar_decisoes, salvo indicação) |
|---|---|
| Número de processo | busca.modo textual, com o número em consulta, sem frase_exata nem cursor |
| ECLI ou outro identificador literal | busca.modo expressao, com âncora literal |
| Trecho citado entre aspas | busca.modo textual com filtros.frase_exata verdadeiro |
| Tese sem número | busca.modo semantica e depois hibrida |
| Acórdão uniformizador | pesquisar_precedentes com busca.filtros.tipo auj; pelo número, pesquisar_decisoes |

Exemplo para um trecho entre aspas:

```json
{"busca": {"modo": "textual", "consulta": "abuso de direito na modalidade de venire contra factum proprium", "filtros": {"frase_exata": true}}}
```

Construa busca segundo o modo e os filtros portugueses que esse modo aceita (ver pesquisar-jurisprudencia-pt).

## Veredictos

- CONFIRMADA: encontrou o registo e os campos devolvidos sustentam a referência e a afirmação.
- DIVERGENTE: encontrou o registo, mas um dado devolvido contradiz a afirmação; indique precisamente a diferença.
- NÃO LOCALIZADA: uma pesquisa concluída não encontrou a referência. Significa apenas que não foi localizada neste pedido e recorte, não que a citação seja necessariamente fabricada.
- NÃO VERIFICÁVEL: erro, timeout, resposta parcial, medida indisponível ou excerto insuficiente para comparar a afirmação.
- FORA DE ÂMBITO: legislação, doutrina ou jurisprudência de outro ordenamento.

Reaja aos sinais da pesquisa como em pesquisar-jurisprudencia-pt: input_contract_refused, corrija a entrada; filtro_nao_suportado ou filtros_ignorados, preserve a restrição e informe que ela não foi aplicada. Só repita por modalidade ou fonte suportada com recorte equivalente; remover ou enfraquecer a restrição exige a escolha do utilizador; parcial com janela_limitada, continue pelo cursor; incompleta com falha_busca ou timeout, repita uma vez mais estreito e, persistindo, declare NÃO VERIFICÁVEL, nunca NÃO LOCALIZADA. Um acórdão que apenas cita o número não é o processo citado.

Os dados de julgamento (relator, vencidos, votos, resultado, tese, data) só valem se vierem de um excerto devolvido nesta conversa; sem ele, escreva «não localizei esse dado no excerto do acórdão». Se o utilizador contestar o veredicto, volte a consultar pelo número completo e cite o excerto.

Nunca confirme de memória. Esta ligação não tem ferramenta para ler a íntegra da decisão, para relações ou citações entre acórdãos, nem para o histórico de redações de um precedente: não infira vigência, força ou relações a partir da presença de um registo e, se a afirmação depender disso, declare NÃO VERIFICÁVEL. Se o serviço devolver uma ligação de origem, preserve-a e descreva a origem como indicada; não invente links nem apresente automaticamente a fonte como oficial.

No final, conte cada veredicto separadamente e destaque as citações não localizadas ou não verificáveis.
