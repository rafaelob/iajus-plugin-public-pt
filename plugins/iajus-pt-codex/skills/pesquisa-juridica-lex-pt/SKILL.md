---
name: pesquisa-juridica-lex-pt
description: "Usa a pesquisa jurídica Lex regular ou profunda apenas quando as ferramentas correspondentes estão expostas nesta ligação."
allowed-tools: mcp__iajus-pt__pesquisa_juridica_lex, mcp__plugin_iajus-pt_iajus-pt__pesquisa_juridica_lex, mcp__iajus-pt__pesquisa_profunda_lex, mcp__plugin_iajus-pt_iajus-pt__pesquisa_profunda_lex, mcp__iajus-pt__consultar_tarefa_lex, mcp__plugin_iajus-pt_iajus-pt__consultar_tarefa_lex, mcp__iajus-pt__abrir_painel_iajus, mcp__plugin_iajus-pt_iajus-pt__abrir_painel_iajus
---

# Pesquisa jurídica Lex (Portugal)

No Codex, se as ferramentas IAJUS não estiverem listadas ou toda chamada voltar 401 ou não autorizado, peça ao utilizador que execute `codex mcp login iajus-pt` e reinicie a sessão; entretanto, não responda de memória.

Use apenas ferramentas que apareçam na lista desta ligação e respeite os respetivos esquemas. A pesquisa Lex não está disponível só por esta skill existir.

## Preparar a consulta

Envie a pergunta jurídica completa em consulta. Acrescente em contexto os factos, a posição processual, as datas e os elementos que alteram a análise. Use restricoes para delimitar tribunais, fontes, período, território ou enfoque. A ausência de restrições não autoriza a ampliar um recorte pedido.

consulta aceita até 8.000 caracteres, contexto até 8.000 e restricoes até 4.000. Os campos são rejeitados se excederem o limite; não há truncagem silenciosa. Se a consulta for longa demais, preserve os elementos determinantes ao reformulá-la ou divida-a em perguntas deliberadas.

Um número de processo ou ECLI citado pelo utilizador vai inteiro em consulta, na forma escrita; não o complete. Recusa de entrada: reformule como a mensagem indicar. Falha ou timeout: diga que a pesquisa FALHOU, nunca «não encontrado». Os dados de um julgamento (relator, vencidos, votos, resultado, tese, data) só valem se vierem de um excerto que o Lex devolveu nesta conversa; sem ele, diga «não localizei esse dado no excerto do acórdão». Se nada voltou, não responda quanto ao mérito: diga que não localizou e ofereça nova pesquisa. Se o utilizador contestar, volte a consultar pelo número completo e cite o excerto.

O Lex exclui doutrina por defeito; não acrescente essa restrição à pergunta. Se o utilizador pedir doutrina, preserve o pedido e informe a indisponibilidade devolvida pelo serviço: esta superfície ainda não a oferece. Apresente as fontes, cobertura e limitações exatamente como a resposta as devolve.

## Execução e repetição

pesquisa_juridica_lex é síncrona: aguarde a resposta desta chamada. pesquisa_profunda_lex inicia uma pesquisa assíncrona e devolve o ID duradouro consulta_id; use consultar_tarefa_lex para obter o estado ou resultado desse ID. cancelar_tarefa_lex solicita cancelamento; um pedido de cancelamento não prova que a execução já parou.

A entrada exige chave_idempotencia. Se o resultado de uma chamada for desconhecido, repita exatamente os mesmos argumentos com a mesma chave. Não reutilize a chave com consulta ou argumentos diferentes; uma nova pergunta ou continuação deliberada recebe uma nova chave.

Se pesquisa_profunda_lex estiver exposta mas consultar_tarefa_lex não estiver, não inicie uma tarefa que não consegue acompanhar. Não simule Events ou tarefas nativas do host, e não prometa execução em segundo plano pelo ChatGPT, Codex ou Claude.

## Continuidade e limites

Use o conversa_id devolvido apenas para continuar a conversa Lex correspondente ou, a pedido do utilizador, para a apagar. A conversa pode ser continuada durante 168 horas depois da criação; leituras e novas mensagens não prolongam esse prazo. Fica guardada até ser apagada com apagar_conversa_lex. O histórico Lex é separado do Studio: não use um ID ou histórico do Studio como conversa_id.

As quotas beta do Lex são próprias por utilizador e categoria: pesquisa comum, 3 por dia e 10 por semana; pesquisa profunda, 1 por dia e 7 por semana. Não tente contornar uma quota com repetições ou pesquisas paralelas.

abrir_painel_iajus abre o painel da ligação; não inicia pesquisa por si só. Não prometa análise de peças, leitura integral de documentos ou capacidades que não constem das ferramentas e resultados expostos.

## Apagar uma conversa

Use apagar_conversa_lex só quando o utilizador pedir expressamente para apagar uma conversa Lex, com o conversa_id devolvido por uma pesquisa desta conta; se o pedido não identificar com clareza a conversa, pergunte antes de chamar. A eliminação é definitiva e não pode ser desfeita, vale também depois das 168 horas de continuação e atinge apenas a conversa indicada. O estado apagada confirma a eliminação. recurso_indisponivel indica que a conversa não existe ou não pertence a esta conta; uma conversa já apagada responde o mesmo, por isso repetir o pedido é seguro. Não apague conversas por iniciativa própria, para refazer uma pesquisa ou para limpar o histórico sem pedido do utilizador.
