# IAJUS - Jurisprudência PT (plugin Codex)

## Licença

O código e as skills deste pacote são licenciados sob Apache-2.0. Consulte [`LICENSE`](LICENSE) e [`NOTICE`](NOTICE) para o escopo do pacote e os componentes excluídos.

Pesquisa e cita **jurisprudência portuguesa real** no Codex, pelo servidor MCP
remoto da IAJUS. As skills orientam o agente a conferir o registo e a proveniência da ligação,
e a nunca citar de memória.

## O que inclui

- **Servidor MCP remoto** `iajus-pt` (`https://pt.mcp.iajus.com.br/mcp`) com `pesquisar_decisoes`, `pesquisar_precedentes` e `consultar_acervo` para a jurisprudência PT.
- **Quatro skills pt-PT:**
  - `pesquisar-jurisprudencia-pt` - acórdãos do STJ, Tribunais da Relação (Lisboa, Porto,
    Coimbra, Évora, Guimarães), STA, TCA-Sul e Norte, Tribunal Constitucional e Conflitos
    via DGSI, e Tribunal de Contas via TCJure; acórdão uniformizador de jurisprudência. Pesquisável por
    descritor ou por ECLI escrito no texto; o resultado traz a ligação de origem, e o ECLI não
    vem no envelope.
  - `verificar-citacoes-pt` - veredicto por citação (CONFIRMADA / DIVERGENTE / NÃO LOCALIZADA /
    FORA DE ÂMBITO) contra o registo e a ligação de origem.
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

## Como ligar

A autenticação é **OAuth 2.1** e o `.mcp.json` deste plugin já aponta o host PT. O Codex só
abre o início de sessão sozinho quando o plugin é instalado pela interface (`/plugins` na TUI,
app ou IDE). Instalado pela linha de comandos (`codex plugin add`), execute uma vez:

```bash
codex mcp login iajus-pt
```

O navegador abre em `app.iajus.com.br`; o token fica guardado no Codex e renova-se sozinho.
Um **401** significa sessão OAuth em falta ou expirada: execute o comando de novo e reinicie a
sessão.

## Dados, privacidade e suporte

- **O que o IAJUS conserva no servidor:** nas ferramentas de consulta ao acervo
  (`pesquisar_decisoes`, `pesquisar_precedentes`, `consultar_acervo`), o texto da consulta pode
  ser conservado após um filtro automático de dados pessoais e credenciais. Esse filtro não
  garante anonimização. Também ficam registos de utilização (ferramenta, recortes como o
  tribunal e o ano, data, hora e resultado). Na pesquisa por sentido, o texto pode ser enviado a
  subcontratantes externos de vetores (OpenAI ou Google) apenas para gerar o vetor de pesquisa.
- **Política de privacidade:** <https://iajus.pt/privacidade>.
- Não inclua dados pessoais, credenciais nem outros dados sensíveis nas consultas.

## Notas

- O conector pesquisa em seu nome e nem toda tool é só de leitura. `pesquisar_decisoes` e
  `consultar_acervo` só leem; `pesquisar_precedentes` pode encaminhar registos de baixa confiança
  para nova verificação.
- Vocabulário e direito **português** (não brasileiro): acórdão, Tribunal da Relação, DGSI,
  ECLI, descritores, acórdão uniformizador de jurisprudência.

Homepage: https://iajus.pt
