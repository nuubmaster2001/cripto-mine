# Cripto Mine

Scanner de gemas low-cap com **Gem Score** — descobre, pontua e rankeia tokens de baixa capitalização com potencial de alta valorização, direto no navegador, **sem chave de API e sem backend**.

## O que faz

- Varre tokens recém-perfilados e em alta no **DexScreener** e pools novos/quentes por rede no **GeckoTerminal**.
- Filtra pela sua faixa (market cap, liquidez mínima, idade, rede) e calcula um **Gem Score** transparente e ajustável.
- Sinais: aceleração de volume (ignição), escada de momentum (5m/1h/6h/24h), FDV/diluição, liquidez para saída.
- Painel de **narrativas em alta** (IA, Meme, DeFi, RWA, DePIN, GameFi…) e categoria por token.
- Inspeção de contrato colado, watchlist persistente, exportação CSV, e link de segurança direto pro GMGN.

## Como rodar

É um único arquivo estático. Basta abrir `index.html` no navegador — ou hospedar (veja abaixo). Hospedado funciona melhor, porque as APIs públicas liberam as chamadas a partir de um site real.

## Fontes de dados

- [DexScreener API](https://docs.dexscreener.com) — pública, sem chave.
- [GeckoTerminal API](https://www.geckoterminal.com/dex-api) — pública, sem chave.

## Deploy (Netlify)

Site estático, sem etapa de build:

- Build command: *(vazio)*
- Publish directory: *(raiz)*

## Aviso

Ferramenta de análise e gestão de risco — **não é recomendação financeira**. Nenhum score prevê valorização, e a checagem de segurança do contrato é sua responsabilidade (use o botão do GMGN). Decisões e capital são seus.
