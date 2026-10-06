---
name: estado-corpus-pt
description: "Consulta a cobertura do acervo português da IAJUS e distingue zero medido de secção não medida ou falha."
---

# Estado do acervo português (IAJUS)

No Codex, se as ferramentas IAJUS não estiverem listadas ou toda chamada voltar 401 ou não autorizado, peça ao utilizador que execute `codex mcp login iajus-pt` e reinicie a sessão; entretanto, não responda de memória.

Antes de consultar, veja quais ferramentas esta ligação expõe. Use apenas uma ferramenta presente na lista e siga o respetivo esquema. Se consultar_acervo estiver disponível, use o ramo abaixo; se só estiver exposta a interface legada obter_estatisticas_base, use os argumentos que essa ferramenta anuncia. Não misture as duas formas de entrada nem suponha que uma ferramenta ausente está disponível.

Use esta consulta para interpretar cobertura, atualidade ou uma pesquisa sem resultados. Não transforme um resultado vazio de pesquisa num retrato do acervo.

## Com consultar_acervo

Em consultar_acervo, use consulta.filtros.secao para escolher uma secção PT: tudo, familias, contadores, orgaos ou qualificadas. limite aceita 1 a 500 e, por defeito, é 60; só limita o ranking de tudo e orgaos, nunca os totais.

Leia o estado e as_of de cada secção separadamente. Informe quais secções foram medidas e o respetivo instante indicado. Uma secção ausente não foi medida nesta chamada.

## Interpretação

- Só reporte zero medido quando a secção respondeu com medição concluída e total igual a zero.
- Um indicador nao_medido ou NOT_MEASURED, uma secção ausente, um erro ou um timeout não significam zero.
- Se a resposta for parcial, identifique as secções ou partes que ficaram por medir. Uma secção bem-sucedida não valida as restantes.
- Trate as_of como o instante daquela medição. Não afirme atualidade, total final ou completude que a resposta não confirma.

Reporte os valores e proveniência exatamente como foram devolvidos. A cobertura não prova acesso à íntegra de cada documento nem cobertura de fontes que não aparecem na resposta.
