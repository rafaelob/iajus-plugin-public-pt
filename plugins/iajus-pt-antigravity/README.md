# IAJUS - Jurisprudência PT

Pesquise jurisprudência portuguesa, confira citações e consulte a cobertura do acervo, preservando a proveniência e as ligações de origem disponíveis.

Plugin nativo do Google Antigravity, versão 2.0.25. Liga ao servidor MCP remoto IAJUS de Portugal, em https://pt.mcp.iajus.com.br/mcp, e requer uma conta IAJUS com acesso ao serviço português. Os resultados preservam a proveniência devolvida, a data e a ligação de origem quando disponíveis. Uma ligação de origem só é apresentada como oficial quando o respetivo registo o comprovar.

## O que inclui o pacote

- O servidor MCP `iajus-pt` (Streamable HTTP), que o Antigravity identifica como `iajus-pt_iajus-pt`.
- As skills de pesquisa jurídica, que também podem ser chamadas através de `/<nome>`:

  - `estado-corpus-pt`: Consulta a cobertura do acervo português da IAJUS com consultar_acervo e distingue zero medido de secção não medida ou falha.
  - `fora-de-ambito-pt`: Distingue pesquisa jurisprudencial portuguesa de legislação, vigência, doutrina, leitura da íntegra e relações entre decisões, capacidades não registadas nesta ligação.
  - `pesquisar-jurisprudencia-pt`: Pesquisa jurisprudência portuguesa com pesquisar_decisoes e pesquisar_precedentes, escolhendo a modalidade e os filtros que cada uma aceita e preservando a proveniência.
  - `verificar-citacoes-pt`: Confere citações de jurisprudência portuguesa uma a uma contra os registos e fontes devolvidos por pesquisar_decisoes e pesquisar_precedentes.

As skills utilizam apenas as ferramentas que a ligação disponibilizar para a sua conta. Uma ferramenta ausente é indicada como indisponível, nunca substituída por outra.

## Instalação

Valide e instale a pasta do plugin com o CLI do Antigravity:

```bash
agy plugin validate --strict ./iajus-pt-antigravity
agy plugin install ./iajus-pt-antigravity
agy plugin list --json
```

Para remover, utilize `agy plugin uninstall iajus-pt`. Para mudar de versão numa instalação feita a partir de uma pasta ou de um repositório, desinstale o plugin e instale a pasta da nova versão.

Estes comandos instalam a pasta local produzida pelo gerador. A existência deste kit não comprova publicação num repositório público ou no Marketplace, nem validação nativa, autenticação ou consultas no Antigravity.

## Início de sessão

O plugin não inclui credenciais. Quando o servidor estabelece a primeira ligação, o Antigravity descobre a autenticação IAJUS (OAuth 2.1) e abre-a no navegador: inicie sessão com a sua conta IAJUS. Se a sessão expirar ou a ligação voltar a pedir autenticação, abra Agent settings, separador Customizations, e utilize Authenticate junto do servidor IAJUS PT deste plugin. Nunca cole tokens, palavras-passe ou chaves na conversa nem nos ficheiros do plugin.

## Limites

- Zero resultados, ou o fim da janela de uma pesquisa, não provam que a decisão não existe.
- A cobertura do acervo é a que as ferramentas de cobertura declaram, com a data da medição.
- A ligação serve jurisprudência portuguesa; não fornece legislação, vigência de normas, doutrina, leitura da íntegra ou relações entre decisões.
- Este pacote não inclui pesquisa Lex nem jurisprudência brasileira. Uma ferramenta indisponível não é substituída pelo serviço brasileiro.
- O plugin não submete peças, não assina nem altera processos e não pesquisa pessoas através de dados privados.

## Suporte e políticas

- [Site](https://iajus.pt)
- [Suporte](https://iajus.pt/contacto.html)
- [Política de privacidade](https://iajus.pt/privacidade)
- [Termos de utilização](https://iajus.pt/termos)

Os ficheiros deste pacote seguem a licença Apache-2.0 (consulte LICENSE e NOTICE). O servidor MCP IAJUS, as APIs e o acervo jurídico não são licenciados por este pacote; o acesso segue os planos, a autenticação, os limites e os termos do IAJUS.
