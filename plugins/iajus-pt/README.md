# IAJUS - Jurisprudência PT (plugin Claude Code)

## Licença

O código e as instruções de pesquisa deste pacote são licenciados sob Apache-2.0. Consulte [`LICENSE`](LICENSE) e [`NOTICE`](NOTICE) para o âmbito do pacote e os componentes excluídos.

A licença deste pacote abrange apenas os ficheiros do próprio pacote. O servidor MCP IAJUS, as suas APIs, as fontes e o corpus jurídico que o sustentam são proprietários da Celeris Juris Tecnologia e Inteligência Jurídica LTDA e não são licenciados por este pacote. O acesso segue os planos, a autenticação, os limites e os termos IAJUS.

Pesquise e cite **jurisprudência portuguesa** diretamente no Claude
(Claude Desktop, claude.ai e Claude Code), através do servidor MCP remoto da IAJUS. As instruções
de pesquisa orientam o Claude a verificar o registo e a proveniência da ligação e a nunca citar de memória.

## O que inclui

- **Servidor MCP remoto** `iajus-pt` (`https://pt.mcp.iajus.com.br/mcp`) com `pesquisar_decisoes`, `pesquisar_precedentes` e `consultar_acervo` para a jurisprudência PT e ligações de origem.
- **Quatro instruções de pesquisa em português de Portugal** que ensinam o Claude a escolher
  a modalidade adequada, aprofundar a pesquisa e verificar cada citação antes de responder:
  - `pesquisar-jurisprudencia-pt` - acórdãos do STJ, Tribunais da Relação (Lisboa, Porto,
    Coimbra, Évora, Guimarães), STA, TCA-Sul e Norte, Tribunal Constitucional e Conflitos
    via DGSI, e Tribunal de Contas via TCJure; acórdão uniformizador de jurisprudência (AUJ). Pesquisável
    por descritor ou por ECLI escrito no texto; o resultado traz a ligação de origem, e o ECLI
    não vem no envelope.
  - `verificar-citacoes-pt` - confere as citações de jurisprudência de uma peça contra a fonte,
    com veredicto por citação (CONFIRMADA / DIVERGENTE / NÃO LOCALIZADA / FORA DE ÂMBITO).
  - `estado-corpus-pt` - a cobertura do acervo PT agora (`consultar_acervo`), com o instante de cada medição.
  - `fora-de-ambito-pt` - o que esta superfície NÃO serve (legislação, doutrina, vigência, íntegra
    da decisão, relações entre decisões) e como responder sem citar direito português de memória.

## Fontes

- **Jurisprudência:** DGSI (www.dgsi.pt) - bases dos tribunais superiores e das Relações.
  Para o Tribunal Constitucional, `dgsi.pt/atco1` é um **espelho histórico**; o portal em
  `tribunalconstitucional.pt` é a fonte oficial. A skill conserva este rótulo de proveniência
  em vez de presumir que toda ligação DGSI tem a mesma natureza.
- **Tribunal de Contas:** TCJure (`tcjure.tcontas.pt`), não DGSI.

Esta superfície é **de jurisprudência**. Não serve legislação portuguesa, doutrina nem vigência
de normas, e não regista ferramenta para ler a íntegra de uma decisão nem para relações entre
decisões: para o texto de uma lei, a fonte oficial é o Diário da República
(diariodarepublica.pt).

O corpus é VIVO e cresce continuamente; órgãos e anos novos aparecem na pesquisa
sem alteração de plugin.

## Como instalar e ligar

O cliente MCP autentica por si. A autenticação canónica é **OAuth 2.1**: no primeiro uso, o
Claude abre o navegador para iniciar sessão em `app.iajus.com.br`. Não é preciso colar nenhuma
chave.

- **Claude Code:** instale o plugin a partir do marketplace IAJUS.pt; o `.mcp.json` já aponta o
  servidor PT e usa OAuth.

  ```text
  /plugin marketplace add https://github.com/rafaelob/iajus-plugin-public-pt
  /plugin install iajus-pt@iajus-pt
  /reload-plugins
  ```

- **claude.ai e Cowork:** em **Customize > Plugins > Add > Add marketplace**, indique
  `https://github.com/rafaelob/iajus-plugin-public-pt` e instale o `iajus-pt`; depois ligue o
  servidor `iajus-pt` no separador **Connectors** do plugin. O início de sessão OAuth abre no navegador.
- **Só o conector (sem as skills):** adicione um conector com o URL
  `https://pt.mcp.iajus.com.br/mcp`; o início de sessão OAuth abre na primeira utilização.

Um **401** significa sessão OAuth em falta ou expirada: volte a autenticar-se.

## Como autorizar as ferramentas

Nenhum plugin autoriza ferramentas sozinho: a autorização é sempre uma configuração local do
utilizador, e as skills não pré-autorizam nenhuma.

- **Claude Code:** para não aprovar cada chamada, acrescente ao `~/.claude/settings.json`
  (global) ou ao `.claude/settings.local.json` (do projeto):

  ```json
  {
    "permissions": {
      "allow": [
        "mcp__plugin_iajus-pt_iajus-pt__pesquisar_decisoes",
        "mcp__plugin_iajus-pt_iajus-pt__pesquisar_precedentes",
        "mcp__plugin_iajus-pt_iajus-pt__consultar_acervo"
      ]
    }
  }
  ```

  Uma ferramenta de plugin chama-se `mcp__plugin_<plugin>_<servidor>__<ferramenta>`; aqui o
  plugin e o servidor chamam-se ambos `iajus-pt`. O exemplo permite as três ferramentas do acervo.
  O utilizador pode manter a aprovação de cada chamada conforme as
  [permissões do Claude Code](https://code.claude.com/docs/en/permissions).
- **claude.ai e Cowork:** no separador **Connectors** do plugin, abra o conector `iajus-pt` e
  escolha, ferramenta a ferramenta, quais ficam sempre permitidas.

## Dados, privacidade e suporte

- **O que o plugin envia:** só as chamadas de ferramenta que o Claude faz ao servidor MCP
  `https://pt.mcp.iajus.com.br/mcp` (a consulta e os parâmetros de pesquisa), com o token OAuth
  da sessão. O plugin não tem hooks, scripts nem servidor local: não executa comandos, não lê
  ficheiros e não guarda dados na máquina do utilizador.
- **O que o IAJUS conserva no servidor:** nas ferramentas de consulta ao acervo
  (`pesquisar_decisoes`, `pesquisar_precedentes`, `consultar_acervo`), o texto da consulta pode
  ser conservado após um filtro automático de dados pessoais e credenciais. Esse filtro não
  garante anonimização. Também ficam registos de utilização (ferramenta, recortes como o
  tribunal e o ano, data, hora e resultado). Na pesquisa por sentido, o texto pode ser enviado a
  subcontratantes externos de vetores (OpenAI ou Google) apenas para gerar o vetor de pesquisa.
- **Política de privacidade:** <https://iajus.pt/privacidade>.
- **Termos de utilização:** <https://iajus.pt/termos>.
- **Suporte:** <https://iajus.pt/contacto>.
- Não inclua dados pessoais, credenciais nem outros dados sensíveis nas consultas.

## Notas

- O conector pesquisa em seu nome e nem todas as ferramentas são apenas de leitura. `pesquisar_decisoes` e
  `consultar_acervo` só leem; `pesquisar_precedentes` pode encaminhar registos de baixa confiança
  para nova verificação.
- Preserve os acentos e o UTF-8 exatamente como na fonte.
- Vocabulário e direito **português** (não brasileiro): acórdão, Tribunal da Relação, DGSI,
  ECLI, descritores, acórdão uniformizador de jurisprudência.

Página inicial: https://iajus.pt
