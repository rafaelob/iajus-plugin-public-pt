# IAJUS - Jurisprudência PT (plugin Codex)

Pesquisa e cita **jurisprudência portuguesa real** no Codex, pelo servidor MCP
remoto da IAJUS. As skills orientam o agente a conferir o registo e a proveniência da ligação,
e a nunca citar de memória.

## O que inclui

- **Servidor MCP remoto** `iajus-pt` (`https://pt.mcp.iajus.com.br/mcp`).
- **Quatro skills pt-PT:**
  - `pesquisar-jurisprudencia-pt` - acórdãos do STJ, Tribunais da Relação (Lisboa, Porto,
    Coimbra, Évora, Guimarães), STA, TCA-Sul e Norte, Tribunal Constitucional e Conflitos
    via DGSI, e Tribunal de Contas via TCJure; acórdão uniformizador de jurisprudência. Pesquisável por
    descritor ou por ECLI escrito no texto; o resultado traz a ligação de origem, e o ECLI não
    vem no envelope.
  - `verificar-citacoes-pt` - veredicto por citação (CONFIRMADA / DIVERGENTE / NÃO LOCALIZADA /
    FORA DE ÂMBITO) contra o registo e a ligação de origem.
  - `estado-corpus-pt` - o que o corpus PT contém agora (por órgão, ano e acervo).
  - `fora-de-ambito-pt` - o que esta superfície NÃO serve (legislação, doutrina, vigência) e
    como responder sem citar direito português de memória.

## Fontes

- **Jurisprudência:** DGSI (www.dgsi.pt) - bases dos tribunais superiores e das Relações.
  Para o Tribunal Constitucional, `dgsi.pt/atco1` é um **espelho histórico**; o portal em
  `tribunalconstitucional.pt` é a fonte oficial. A skill conserva este rótulo de proveniência
  em vez de presumir que toda ligação DGSI tem a mesma natureza.
- **Tribunal de Contas:** TCJure (`tcjure.tcontas.pt`), não DGSI.

Esta superfície é **de jurisprudência**. Não serve legislação portuguesa, doutrina nem vigência
de normas: para o texto de uma lei, a fonte oficial é o Diário da República
(diariodarepublica.pt).

## Como ligar

O cliente autentica por si. A autenticação canónica é **OAuth 2.1**: no primeiro uso, o Codex
abre o navegador para iniciar sessão em `app.iajus.com.br`. O `.mcp.json` deste plugin já aponta
o host PT e usa OAuth.

**Alternativa (canal privado):** uma chave `ik_*` no cabeçalho `Authorization: Bearer`. Nunca
cole a chave em conversa nem em commit. Um **401** significa sessão ou chave em falta ou
expirada.

## Notas

- Todas as tools são **somente-leitura**: pesquisam e citam, nunca escrevem.
- Vocabulário e direito **português** (não brasileiro): acórdão, Tribunal da Relação, DGSI,
  ECLI, descritores, acórdão uniformizador de jurisprudência.

Homepage: https://iajus.pt
