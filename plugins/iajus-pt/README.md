# IAJUS - Jurisprudência PT (plugin Claude Code)

Pesquisa e cita **jurisprudência portuguesa real** diretamente no Claude
(Claude Desktop, claude.ai e Claude Code), pelo servidor MCP remoto da IAJUS. As skills
orientam o Claude a usar a fonte oficial e a nunca citar de memória.

## O que inclui

- **Servidor MCP remoto** `iajus-pt` (`https://pt.mcp.iajus.com.br/mcp`) com pesquisa e
  leitura de jurisprudência PT.
- **Quatro skills pt-PT** que ensinam o Claude a escolher a modalidade certa, escalar a
  pesquisa e conferir cada citação antes de entregar:
  - `pesquisar-jurisprudencia-pt` - acórdãos do STJ, Tribunais da Relação (Lisboa, Porto,
    Coimbra, Évora, Guimarães), STA, TCA-Sul e Norte, Tribunal Constitucional, Conflitos e
    Tribunal de Contas, via DGSI; acórdão uniformizador de jurisprudência (AUJ). Pesquisável
    por descritor ou por ECLI escrito no texto; o resultado traz o link oficial da DGSI, e o
    ECLI não vem no envelope.
  - `verificar-citacoes-pt` - confere as citações de jurisprudência de uma peça contra a fonte,
    com veredicto por citação (CONFIRMADA / DIVERGENTE / NÃO LOCALIZADA / FORA DE ÂMBITO).
  - `estado-corpus-pt` - o que o corpus PT contém agora (por órgão, ano e acervo).
  - `fora-de-ambito-pt` - o que esta superfície NÃO serve (legislação, doutrina, vigência) e
    como responder sem citar direito português de memória.

## Fontes

- **Jurisprudência:** DGSI (www.dgsi.pt) - bases dos tribunais superiores e das Relações.

Esta superfície é **de jurisprudência**. Não serve legislação portuguesa, doutrina nem vigência
de normas: para o texto de uma lei, a fonte oficial é o Diário da República
(diariodarepublica.pt).

O corpus é VIVO e cresce continuamente; órgãos e anos novos aparecem na pesquisa
sem alteração de plugin.

## Como ligar

O cliente MCP autentica por si. A autenticação canónica é **OAuth 2.1**: no primeiro uso, o
Claude abre o navegador para iniciar sessão em `app.iajus.com.br`. Não é preciso colar nenhuma
chave.

- **Claude Desktop / claude.ai:** adicione o conector com o URL
  `https://pt.mcp.iajus.com.br/mcp`; o login OAuth abre no primeiro uso.
- **Claude Code:** instale o plugin a partir do marketplace IAJUS; o `.mcp.json` já aponta o
  host PT e usa OAuth.

**Alternativa (canal privado/CLI):** uma chave `ik_*` no cabeçalho `Authorization: Bearer`.
Nunca cole a chave em conversa nem em commit. Um **401** significa sessão ou chave em falta ou
expirada: refaça o login ou reveja a chave.

## Notas

- Todas as tools são **somente-leitura**: pesquisam e citam, nunca escrevem.
- Preserve os acentos e o UTF-8 exatamente como na fonte.
- Vocabulário e direito **português** (não brasileiro): acórdão, Tribunal da Relação, DGSI,
  ECLI, descritores, acórdão uniformizador de jurisprudência.

Homepage: https://iajus.pt
