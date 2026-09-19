# IAJUS Portugal: jurisprudência portuguesa no seu assistente de IA

Este é o marketplace oficial do plugin **IAJUS Portugal**. Ele liga o seu assistente (Claude Code, Codex ou ChatGPT) à base portuguesa do IAJUS: acórdãos dos tribunais superiores e das Relações via DGSI, acórdão uniformizador de jurisprudência e a verificação de cada citação contra o registo e a ligação de origem. O plugin existe para o seu assistente **citar o que existe de verdade**, nunca inventar precedente.

> Procura jurisprudência e legislação do **Brasil**? Esse é um marketplace separado: [github.com/rafaelob/iajus-plugin-public](https://github.com/rafaelob/iajus-plugin-public).

## Migração de quem instalou antes da separação

Até 2026-07-23 o plugin de Portugal era distribuído no marketplace do Brasil. Se adicionou o marketplace antes dessa data, remova o marketplace antigo e adicione o desta jurisdição. No Claude Code:

```text
/plugin marketplace remove iajus
/plugin marketplace add https://github.com/rafaelob/iajus-plugin-public-pt
/plugin install iajus-pt@iajus-pt
/plugin enable iajus-pt@iajus-pt
/reload-plugins
```

No Codex:

```bash
codex plugin marketplace remove iajus
codex plugin marketplace add https://github.com/rafaelob/iajus-plugin-public-pt
codex plugin add iajus-pt@iajus-pt
```

## O que ganha

- **Pesquisa de jurisprudência portuguesa** em modalidades complementares (multimodal, semântica e por identificador), sempre com sumário e ligação de origem identificada.
- **Proveniência sem rótulos enganosos**: em resultados do Tribunal Constitucional, `dgsi.pt/atco1` é um espelho histórico; o portal em `tribunalconstitucional.pt` é a fonte oficial. Nos restantes órgãos, a ligação é apresentada como origem, sem presumir oficialidade apenas pelo domínio.
- **Precedente qualificado**: os acórdãos uniformizadores de jurisprudência (AUJ) do Supremo Tribunal de Justiça, citáveis pelo teor. A base **não regista a vigência** destes acórdãos e o plugin não a afirma: confirme o estado na fonte competente indicada para o órgão.
- **Estado do corpus ao vivo**: confirme a cobertura atual antes de concluir que não há resultados.
- **Skills pt-PT prontas** que ensinam o assistente o método de pesquisa jurídica portuguesa.

## Instalar

### Claude Code (comandos, um de cada vez)

```text
/plugin marketplace add https://github.com/rafaelob/iajus-plugin-public-pt
/plugin install iajus-pt@iajus-pt
/plugin enable iajus-pt@iajus-pt
/reload-plugins
```

O login OAuth abre no navegador na primeira ligação: entre com a sua conta IAJUS e pronto.

### Codex

```bash
codex plugin marketplace add https://github.com/rafaelob/iajus-plugin-public-pt
codex plugin add iajus-pt@iajus-pt
```

### Outros assistentes (ligação direta ao servidor MCP)

Assistentes que aceitam um servidor MCP por URL ligam-se ao IAJUS Portugal sem plugin, apontando para `https://pt.mcp.iajus.com.br/mcp` e autenticando por **OAuth 2.1** (login no navegador). Ganha as mesmas ferramentas de pesquisa jurídica; as skills prontas não acompanham a ligação direta.

## Conta e acesso

Crie a sua conta em [iajus.pt](https://iajus.pt). A autenticação padrão é **OAuth 2.1** (login no navegador, renovação automática): nunca precisa de colar token em ficheiro. Detalhes de cada pacote, incluindo a alternativa manual por chave, estão nos READMEs de `plugins/iajus-pt` (Claude Code) e `plugins/iajus-pt-codex` (Codex).

## Suporte

Dúvidas ou problemas: [iajus.pt](https://iajus.pt) ou contato@iajus.com.br.

Política de privacidade: [iajus.pt/privacidade](https://iajus.pt/privacidade). Termos de uso: [iajus.pt/termos](https://iajus.pt/termos).
