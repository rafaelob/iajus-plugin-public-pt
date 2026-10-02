---
name: pesquisa-juridica-lex-pt
description: "Usa a pesquisa jurídica Lex regular ou profunda apenas quando as ferramentas correspondentes estão expostas nesta ligação."
---

# Pesquisa jurídica Lex (Portugal)

Use apenas ferramentas que apareçam na lista desta ligação e respeite os respetivos esquemas. A pesquisa Lex não está disponível só por esta skill existir.

## Preparar a consulta

Envie a pergunta jurídica completa em consulta. Acrescente em contexto os factos, a posição processual, as datas e os elementos que alteram a análise. Use restricoes para delimitar tribunais, fontes, período, território ou enfoque. A ausência de restrições não autoriza a ampliar um recorte pedido.

consulta aceita até 8.000 caracteres, contexto até 8.000 e restricoes até 4.000. Os campos são rejeitados se excederem o limite; não há truncagem silenciosa. Se a consulta for longa demais, preserve os elementos determinantes ao reformulá-la ou divida-a em perguntas deliberadas.

O Lex exclui doutrina por defeito; não acrescente essa restrição à pergunta. Se o utilizador pedir doutrina, preserve o pedido e informe a indisponibilidade devolvida pelo serviço: esta superfície ainda não a oferece. Apresente as fontes, cobertura e limitações exatamente como a resposta as devolve.

## Execução e repetição

pesquisa_juridica_lex é síncrona: aguarde a resposta desta chamada. pesquisa_profunda_lex inicia uma pesquisa assíncrona e devolve o ID duradouro consulta_id; use consultar_tarefa_lex para obter o estado ou resultado desse ID. cancelar_tarefa_lex solicita cancelamento; um pedido de cancelamento não prova que a execução já parou.

A entrada exige chave_idempotencia. Se o resultado de uma chamada for desconhecido, repita exatamente os mesmos argumentos com a mesma chave. Não reutilize a chave com consulta ou argumentos diferentes; uma nova pergunta ou continuação deliberada recebe uma nova chave.

Se pesquisa_profunda_lex estiver exposta mas consultar_tarefa_lex não estiver, não inicie uma tarefa que não consegue acompanhar. Não simule Events ou tarefas nativas do host, e não prometa execução em segundo plano pelo ChatGPT, Codex ou Claude.

## Continuidade e limites

Use o conversa_id devolvido apenas para continuar a conversa Lex correspondente. Expira 168 horas depois da criação; leituras e novas mensagens não prolongam esse prazo. O histórico Lex é separado do Studio: não use um ID ou histórico do Studio como conversa_id.

As quotas beta do Lex são próprias por utilizador e categoria: pesquisa comum, 3 por dia e 10 por semana; pesquisa profunda, 1 por dia e 7 por semana. Não tente contornar uma quota com repetições ou pesquisas paralelas.

abrir_painel_iajus abre o painel da ligação; não inicia pesquisa por si só. Não prometa análise de peças, leitura integral de documentos ou capacidades que não constem das ferramentas e resultados expostos.
