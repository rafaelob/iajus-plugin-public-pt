---
name: verificar-citacoes-pt
description: 'Confere uma a uma as citações de jurisprudência PORTUGUESA de um texto (peça, parecer, alegações, minuta) contra a fonte oficial pelo MCP IAJUS, com veredicto por citação: CONFIRMADA / DIVERGENTE / NÃO LOCALIZADA / FORA DE ÂMBITO. Accione em "confira os acórdãos citados", "este acórdão existe ou foi inventado?", "valide os precedentes desta peça". É o antídoto da alucinação de citação. NÃO pesquisa de raiz (use pesquisar-jurisprudencia-pt) e NÃO confere normas nem vigência.'
allowed-tools: mcp__iajus-pt__buscar_regex, mcp__plugin_iajus-pt_iajus-pt__buscar_regex, mcp__iajus-pt__buscar_fts, mcp__plugin_iajus-pt_iajus-pt__buscar_fts, mcp__iajus-pt__buscar_hibrida, mcp__plugin_iajus-pt_iajus-pt__buscar_hibrida, mcp__iajus-pt__buscar_semantica, mcp__plugin_iajus-pt_iajus-pt__buscar_semantica, mcp__iajus-pt__buscar_qualificada, mcp__plugin_iajus-pt_iajus-pt__buscar_qualificada
---

# Verificar citações de jurisprudência portuguesa (IAJUS)

Tem acesso ao servidor MCP `iajus-pt` para **conferir, uma a uma, se as citações de
jurisprudência portuguesa de um texto existem mesmo e dizem o que o texto lhes atribui**,
batendo cada uma contra a fonte oficial (DGSI, mais o Tribunal de Contas). É o antídoto do erro
mais perigoso de um texto jurídico gerado por IA: o acórdão que não existe, o número trocado, o
sumário que ninguém escreveu. O produto é um **veredicto por citação**, ancorado no que a fonte
devolveu: **nunca se confirma de memória.**

Accione esta skill quando pedirem "confira os acórdãos citados nesta peça", "este acórdão é real
ou foi inventado?", "valide os precedentes". Para pesquisar de raiz - achar precedentes que ainda
não estão no texto - use a skill `pesquisar-jurisprudencia-pt`.

## Envelope de desfecho

Leia a chave `desfecho` ANTES de qualquer contagem. Os cinco valores são mutuamente exclusivos:

- `erro` — a consulta FALHOU; ninguém consultou o acervo. Não é ausência.
- `sem_resultado` — a consulta CORREU e o acervo não tem. Zero MEDIDO.
- `nao_terminou` — tempo esgotado ou tecto. NÃO-MEDIDO; não afirme que «não existe».
- `parcial` — mediu uma parte; declare o que ficou de fora.
- `medida_indisponivel` - a fonte respondeu e NÃO carrega a medida. NÃO-MEDIDO sem avaria; não é zero.

`total: 0` só é ausência medida quando `desfecho` é `sem_resultado`. Sem `desfecho`, ou com `erro`/`nao_terminou`, a citação ficou **não verificável por falha** — não NÃO LOCALIZADA.

## O que esta superfície confere, e o que NÃO consegue conferir

Esta é a parte a ler primeiro, porque o mais perigoso aqui não é falhar uma verificação: é
**emitir um veredicto que a base não sustenta**. Um veredicto é uma afirmação sobre o Direito.

**CONFERE (dois eixos):**

1. **Existência** - a citação corresponde a um acórdão real na base, com aquele tribunal, número
   e data.
2. **Fidelidade** - o que o texto afirma sobre o acórdão bate com a fonte: sumário, descritores,
   tribunal, relator, data, secção.

**NÃO CONFERE, e não o encene:**

3. **Vigência.** A base **não regista vigência** de acórdão uniformizador de jurisprudência
   (AUJ): vêm todos com `status_vigencia: "desconhecida"` e a estatística conta 0 vigentes e 0
   canceladas. Um zero ali é **não medido**, nunca "nenhum está vigente". **Nunca emita um
   veredicto de vigência** - nem "ainda em vigor", nem "superado", nem "revogado". Se o
   utilizador perguntar isso, diga que esta superfície não o mede e remeta à fonte oficial.
4. **Citações de legislação.** Um artigo do Código Civil, um decreto-lei, uma portaria: esta
   superfície **não serve legislação portuguesa** e não há como bater a redação contra a fonte.
   O veredicto é **FORA DE ÂMBITO** - e a regra que aqui vale mais do que qualquer outra é
   **nunca citar nem parafrasear de memória o texto de uma norma portuguesa** para preencher a
   lacuna. Ver a skill `fora-de-ambito-pt`.
5. **Quem cita quem.** Não há grafo de citações entre decisões portuguesas, portanto não se
   consegue confirmar que um acórdão posterior acolheu ou afastou o precedente citado.

## Como conferir, por tipo de citação

| Citação no texto | Tool | O que conferir |
|---|---|---|
| Número de processo, ECLI ou referência da DGSI | `buscar_regex` | Procure a referência **tal como está escrita**, com um trecho literal de pelo menos 3 caracteres. Depois **confirme contra os campos devolvidos** (`numero_processo`, `tribunal`, `data`) em vez de assumir que o padrão garantiu o acerto. |
| Trecho de sumário citado entre aspas | `buscar_fts` | Texto integral, insensível a acento: procure a expressão tal como o texto a cita. Uma citação entre aspas que não aparece em lado nenhum é o sinal mais forte de fabricação. |
| Acórdão descrito pela tese, sem número | `buscar_semantica` e depois `buscar_hibrida` | Ache o julgado real. Se nada casar depois de escalar, é suspeito de fabricação -> NÃO LOCALIZADA. |
| O texto afirma que existe um acórdão uniformizador (AUJ) sobre a questão | `buscar_qualificada(tipo='auj')` mais as modalidades de texto | Só o Supremo Tribunal de Justiça uniformiza jurisprudência (art. 686.º do CPC): um acórdão de Relação nunca é AUJ, por muito que cite um. **Leia os avisos abaixo antes de usar esta tool.** |

### Avisos obrigatórios de `buscar_qualificada`

- **`buscar_qualificada` NÃO enumera o acervo.** Devolve no máximo **10** por chamada, e `k=50`
  continua a devolver 10. O campo `total` do envelope conta **o que voltou, não o que existe**.
  **Nunca** apresente o que voltou como a lista completa e **nunca** conclua, de um resultado
  vazio ou curto, que o AUJ citado não existe: escale para as modalidades de texto antes de
  emitir NÃO LOCALIZADA.
- **Leia `uniformizador_valido` antes de tratar um resultado como AUJ.** A tabela tem 682 linhas com esta espécie, mas só as **649** do Supremo são AUJ: as outras **33** são acórdãos da Relação de Lisboa que citam ou discutem um AUJ e ficaram com a espécie errada por defeito de classificação da fonte. O servidor serve-as marcadas, com `uniformizador_valido: false`, `citavel_como_precedente: false` e um `aviso_uniformizador` que invoca o art. 686.º do CPC. **O servidor faz a metade dele; esta é a sua.** Confirmar uma dessas como acórdão uniformizador é afirmar que a Relação uniformizou jurisprudência - e é falso. Para receber só as do Supremo, passe `orgao='pt_stj'`.
- **`materia` e `numero` não selecionam AUJ**: o campo por trás de `materia` está a nulo em todos
  os registos e `numero` guarda o identificador documental da DGSI, não o número citável. Para
  conferir "o AUJ n.º 8/2022", procure-o pelas modalidades de texto, nunca por `numero`.

## Método (execute a verificação, não a reescrita)

Cada citação é uma unidade independente. Esgote a busca antes de reprovar qualquer uma delas.

1. **Extraia as citações do texto** e, com cada uma, **o que o texto AFIRMA sobre ela** (tribunal,
   número, data, relator, teor do sumário). É essa afirmação que vai bater contra a fonte, não a
   sua impressão do que o acórdão deveria dizer.
2. **Separe já o que é jurisprudência do que não é.** Citação de norma, de doutrina ou de
   jurisprudência de outro ordenamento sai desta verificação com FORA DE ÂMBITO, declarado ao
   utilizador. Não a verifique "por aproximação".
3. **Bata cada citação contra a fonte** pela tabela acima. Uma citação só se confirma se uma
   chamada REAL ao MCP nesta sessão a devolveu.
4. **Esgote antes de reprovar.** Se a primeira modalidade vier vazia, escale - `buscar_semantica`
   -> `buscar_hibrida` com a mesma consulta -> reformule com os termos que apareceram nos
   primeiros resultados -> `buscar_fts` para o trecho literal -> `buscar_regex` para o número ou
   ECLI - e só então marque NÃO LOCALIZADA. Um acórdão real pode estar sob outra grafia.
5. **Deduplique antes de contar.** Cinco tribunais estão guardados sob duas grafias de chave, pelo
   que o mesmo acórdão pode voltar mais do que uma vez na mesma lista. Deduplique por
   `link_completo` e não confunda duas ocorrências com duas confirmações.
6. **Emita o veredicto por citação** e feche com um resumo.

## Veredicto por citação (formato de saída)

- **CONFIRMADA** - existe e é fiel. Anexe o `link_completo` devolvido pela fonte e o dado que
  confirma (sumário, tribunal, data, relator). Confirmar não é afirmar vigência.
- **DIVERGENTE** - o acórdão existe, mas o texto afirma algo que a fonte contradiz (tribunal,
  número, data, relator, teor do sumário). Diga exatamente o que diverge e qual é o dado da
  fonte, com o link.
- **NÃO LOCALIZADA** - a fonte não devolveu a citação depois da escalada completa. **É um alerta
  de possível alucinação**: reporte como não verificada, nunca como "provavelmente existe".
  Distinga honestamente as duas hipóteses e diga qual os sinais sustentam: (a) citação fabricada;
  (b) limite da base - por exemplo, um acórdão do Tribunal Constitucional posterior a 1998, que é
  fronteira medida da base e não ausência na realidade.
- **FORA DE ÂMBITO** - a citação não é de jurisprudência portuguesa servida por esta superfície
  (norma, doutrina, jurisprudência estrangeira). **Não é um veredicto sobre a citação**: é a
  declaração de que esta via não a alcança. Diga-o e indique a fonte oficial adequada.

Feche com um **resumo**: N confirmadas, N divergentes, N não localizadas, N fora de âmbito - e
destaque as não localizadas, que são as que exigem atenção.

## Regras (inegociáveis)

- **Nunca confirme de memória.** Só é CONFIRMADA a citação que uma chamada ao MCP nesta sessão
  devolveu. Sem retorno, é NÃO LOCALIZADA.
- **Nunca emita veredicto de vigência.** A base não a mede; zero num campo de vigência é não
  medido.
- **Nunca preencha uma lacuna com um precedente plausível.** Um acórdão inventado para fechar o
  relatório é exatamente o defeito que esta skill existe para apanhar.
- **Não reescreva o texto do utilizador.** Entrega-se o veredicto; a correção é decisão do autor.
- **Um número, sumário, relator ou link só entra na resposta se veio da tool.**

## Vocabulário (direito português)

Use: acórdão, Tribunal da Relação, Supremo Tribunal de Justiça, acórdão uniformizador de
jurisprudência (AUJ), descritores, sumário, ECLI, força obrigatória geral. **Não** transponha
instituições, categorias de precedente nem sistemas de classificação de outros ordenamentos: o
utilizador leria isso como direito português.

## Autenticação

O cliente MCP autentica por si: **login OAuth** (o navegador abre no primeiro uso) **ou** chave
`ik_*` no cabeçalho `Authorization: Bearer` (canal privado). Um **401** indica sessão ou chave em
falta ou expirada: peça ao utilizador para refazer o login ou rever a chave; **nunca** cole a
chave em conversa nem em commit. Todas as tools são somente-leitura.
