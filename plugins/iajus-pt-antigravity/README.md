# IAJUS - Jurisprudência PT

Pesquise jurisprudência portuguesa, confira citações e consulte a cobertura do acervo, preservando a proveniência e as ligações de origem disponíveis.

Plugin nativo do Google Antigravity, versão 2.0.24. Liga ao servidor MCP remoto IAJUS de Portugal, em https://pt.mcp.iajus.com.br/mcp, e exige uma conta IAJUS. Os resultados trazem a fonte, a data e o link oficial quando disponíveis.

## O que vem no pacote

- O servidor MCP `iajus-pt` (Streamable HTTP), que o Antigravity identifica como `iajus-pt_iajus-pt`.
- As skills de pesquisa jurídica, que também podem ser chamadas por `/<nome>`:

  - `estado-corpus-pt`: Consulta a cobertura do acervo português da IAJUS com consultar_acervo e distingue zero medido de secção não medida ou falha.
  - `fora-de-ambito-pt`: Distingue pesquisa jurisprudencial portuguesa de legislação, vigência, doutrina, leitura da íntegra e relações entre decisões, capacidades não registadas nesta ligação.
  - `pesquisar-jurisprudencia-pt`: Pesquisa jurisprudência portuguesa com pesquisar_decisoes e pesquisar_precedentes, escolhendo a modalidade e os filtros que cada uma aceita e preservando a proveniência.
  - `verificar-citacoes-pt`: Confere citações de jurisprudência portuguesa uma a uma contra os registos e fontes devolvidos por pesquisar_decisoes e pesquisar_precedentes.

As skills usam somente as ferramentas que a conexão listar para a sua conta. Uma ferramenta ausente é informada como indisponível, nunca substituída por outra.

## Instalação

Valide e instale a pasta do plugin com o CLI do Antigravity:

```bash
agy plugin validate --strict ./iajus-pt-antigravity
agy plugin install ./iajus-pt-antigravity
agy plugin list --json
```

Para remover, use `agy plugin uninstall iajus-pt`. Para trocar de versão numa instalação feita a partir de uma pasta ou de um repositório, desinstale o plugin e instale a pasta da nova versão.

Estes comandos instalam a pasta local produzida pelo gerador. A existência deste kit não comprova publicação num repositório público ou no Marketplace, nem validação nativa, autenticação ou consultas no Antigravity.

## Login

O plugin não traz credenciais. Quando o servidor conecta pela primeira vez, o Antigravity descobre o login IAJUS (OAuth 2.1) e o abre no navegador: entre com a sua conta IAJUS. Se a sessão expirar ou a conexão pedir autenticação de novo, abra Agent settings, aba Customizations, e use Authenticate ao lado do servidor IAJUS deste plugin. Nunca cole tokens, senhas ou chaves no chat nem nos arquivos do plugin.

## Limites

- Zero resultados, ou o fim da janela de uma busca, não provam que a decisão ou a norma não existe.
- A cobertura do acervo é a que as ferramentas de cobertura declaram, com a data da medição.
- A ligação serve jurisprudência portuguesa; não fornece legislação, vigência de normas, doutrina, leitura da íntegra ou relações entre decisões.
- Este pacote não inclui pesquisa Lex nem jurisprudência brasileira. Uma ferramenta indisponível não é substituída pelo serviço brasileiro.
- O plugin não protocola, não assina e não altera processos, e não pesquisa pessoas por dados privados.

## Suporte e políticas

- [Site](https://iajus.pt)
- [Suporte](https://iajus.pt/contacto.html)
- [Política de privacidade](https://iajus.pt/privacidade)
- [Termos de uso](https://iajus.pt/termos)

Os arquivos deste pacote seguem a licença Apache-2.0 (veja LICENSE e NOTICE). O servidor MCP IAJUS, suas APIs e o acervo jurídico não são licenciados por este pacote; o acesso segue os planos, a autenticação, os limites e os termos do IAJUS.
