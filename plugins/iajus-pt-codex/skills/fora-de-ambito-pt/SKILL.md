---
name: fora-de-ambito-pt
description: "Distingue pesquisa jurisprudencial portuguesa de legislação, vigência, doutrina e outras capacidades não expostas nesta ligação."
allowed-tools: mcp__iajus-pt__pesquisar_decisoes, mcp__plugin_iajus-pt_iajus-pt__pesquisar_decisoes, mcp__iajus-pt__pesquisar_precedentes, mcp__plugin_iajus-pt_iajus-pt__pesquisar_precedentes, mcp__iajus-pt__pesquisa_juridica_lex, mcp__plugin_iajus-pt_iajus-pt__pesquisa_juridica_lex, mcp__iajus-pt__buscar_semantica, mcp__plugin_iajus-pt_iajus-pt__buscar_semantica, mcp__iajus-pt__buscar_hibrida, mcp__plugin_iajus-pt_iajus-pt__buscar_hibrida, mcp__iajus-pt__buscar_fts, mcp__plugin_iajus-pt_iajus-pt__buscar_fts, mcp__iajus-pt__buscar_regex, mcp__plugin_iajus-pt_iajus-pt__buscar_regex
---

# Âmbito da ligação portuguesa IAJUS

No Codex, se as ferramentas IAJUS não estiverem listadas ou toda chamada voltar 401 ou não autorizado, peça ao utilizador que execute `codex mcp login iajus-pt` e reinicie a sessão; entretanto, não responda de memória.

Consulte primeiro a lista de ferramentas desta ligação. Use apenas nomes presentes e o esquema próprio de cada um. As instruções abaixo não tornam uma ferramenta disponível.

## Pedido de lei, artigo ou vigência

Se pesquisa_juridica_lex estiver exposta, use-a com a pergunta completa, os factos necessários e as restrições relevantes. Apresente as fontes e os limites devolvidos; não acrescente doutrina por defeito. Para vigência, indique a data relevante na consulta.

Se essa ferramenta não estiver exposta, as ferramentas de decisões e precedentes só permitem oferecer jurisprudência portuguesa efetivamente devolvida. Não as apresente como texto de lei, prova de vigência ou resposta normativa. Encaminhe a verificação para a fonte oficial adequada, como o Diário da República, e não cite direito português de memória.

Em perguntas sobre direitos, deveres ou regime de uma categoria, a norma aplicável é o ponto de partida: se pesquisa_juridica_lex estiver exposta, peça-a por ela, citando o diploma pelo número quando o utilizador o souber; caso contrário, remeta para o Diário da República. Só diga que a norma não foi encontrada se essa consulta foi feita e falhou; a jurisprudência devolvida não substitui a norma.

## Limites

Não presuma que esta ligação permite consultar legislação, doutrina, vigência, íntegra de documentos, versões históricas ou relações entre decisões. Não transporte filtros ou capacidades do perfil brasileiro para Portugal.

Só diga que uma capacidade está disponível se a ferramenta correspondente aparecer nesta ligação. Se não houver uma ferramenta adequada, explique o limite e ofereça a alternativa disponível, sem anunciar funções futuras.

Um zero numa pesquisa concluída significa apenas que não houve resultados para aquele pedido e recorte. Erro, timeout, resposta parcial, secção não medida ou ferramenta indisponível não provam inexistência.
