# IAJUS - Jurisprudência PT (plugin Claude Code)

Pesquisa e cita **jurisprudência portuguesa real** diretamente no Claude
(Claude Desktop, claude.ai e Claude Code), pelo servidor MCP remoto da IAJUS. As skills
orientam o Claude a conferir o registo e a proveniência da ligação, e a nunca citar de memória.

## O que inclui

- **Servidor MCP remoto** `iajus-pt` (`https://pt.mcp.iajus.com.br/mcp`) com pesquisa e
  leitura de jurisprudência PT.
- **Quatro skills pt-PT** que ensinam o Claude a escolher a modalidade certa, escalar a
  pesquisa e conferir cada citação antes de entregar:
  - `pesquisar-jurisprudencia-pt` - acórdãos do STJ, Tribunais da Relação (Lisboa, Porto,
    Coimbra, Évora, Guimarães), STA, TCA-Sul e Norte, Tribunal Constitucional e Conflitos
    via DGSI, e Tribunal de Contas via TCJure; acórdão uniformizador de jurisprudência (AUJ). Pesquisável
    por descritor ou por ECLI escrito no texto; o resultado traz a ligação de origem, e o ECLI
    não vem no envelope.
  - `verificar-citacoes-pt` - confere as citações de jurisprudência de uma peça contra a fonte,
    com veredicto por citação (CONFIRMADA / DIVERGENTE / NÃO LOCALIZADA / FORA DE ÂMBITO).
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

O corpus é VIVO e cresce continuamente; órgãos e anos novos aparecem na pesquisa
sem alteração de plugin.

## Como instalar e ligar

O cliente MCP autentica por si. A autenticação canónica é **OAuth 2.1**: no primeiro uso, o
Claude abre o navegador para iniciar sessão em `app.iajus.com.br`. Não é preciso colar nenhuma
chave.

- **Claude Code:** instale o plugin a partir do marketplace IAJUS.pt; o `.mcp.json` já aponta o
  host PT e usa OAuth.

  ```text
  /plugin marketplace add https://github.com/rafaelob/iajus-plugin-public-pt
  /plugin install iajus-pt@iajus-pt
  /reload-plugins
  ```

- **claude.ai e Cowork:** em **Customize > Plugins > Add > Add marketplace**, indique
  `https://github.com/rafaelob/iajus-plugin-public-pt` e instale o `iajus-pt`; depois ligue o
  servidor `iajus-pt` no separador **Connectors** do plugin. O login OAuth abre no navegador.
- **Só o conector (sem as skills):** adicione um conector com o URL
  `https://pt.mcp.iajus.com.br/mcp`; o login OAuth abre no primeiro uso.

**Alternativa (canal privado/CLI):** uma chave `ik_*` no cabeçalho `Authorization: Bearer`.
Nunca cole a chave em conversa nem em commit. Um **401** significa sessão ou chave em falta ou
expirada: refaça o login ou reveja a chave.

## Como autorizar as ferramentas

Nenhum plugin autoriza ferramentas sozinho: a autorização é sempre uma configuração local do
utilizador, e as skills não pré-autorizam nenhuma.

- **Claude Code:** para não aprovar cada chamada, acrescente ao `~/.claude/settings.json`
  (global) ou ao `.claude/settings.local.json` (do projeto):

  ```json
  { "permissions": { "allow": ["mcp__plugin_iajus-pt_iajus-pt__*"] } }
  ```

  Uma ferramenta de plugin chama-se `mcp__plugin_<plugin>_<servidor>__<ferramenta>`; aqui o
  plugin e o servidor chamam-se ambos `iajus-pt`, e o `*` cobre todas as ferramentas.
- **claude.ai e Cowork:** no separador **Connectors** do plugin, abra o conector `iajus-pt` e
  escolha, ferramenta a ferramenta, quais ficam sempre permitidas.

## Dados, privacidade e suporte

- **O que o plugin envia:** só as chamadas de ferramenta que o Claude faz ao servidor MCP
  `https://pt.mcp.iajus.com.br/mcp` (a consulta e os parâmetros de pesquisa), com o token OAuth
  da sessão. O plugin não tem hooks, scripts nem servidor local: não executa comandos, não lê
  ficheiros e não guarda dados na máquina do utilizador.
- **Política de privacidade:** <https://iajus.pt/privacidade>.
- **Termos de utilização:** <https://iajus.pt/termos>.
- **Suporte:** <https://iajus.pt/contacto>.
- Não inclua dados pessoais, credenciais nem outros dados sensíveis nas consultas.

## Notas

- Todas as tools são **somente-leitura**: pesquisam e citam, nunca escrevem.
- Preserve os acentos e o UTF-8 exatamente como na fonte.
- Vocabulário e direito **português** (não brasileiro): acórdão, Tribunal da Relação, DGSI,
  ECLI, descritores, acórdão uniformizador de jurisprudência.

Homepage: https://iajus.pt
