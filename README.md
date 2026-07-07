# Cripto Mine

Scanner de gemas low-cap com **Gem Score** — descobre, pontua e rankeia tokens de baixa capitalização com potencial de alta valorização, direto no navegador, **sem chave de API e sem backend**. Feito para dois estilos de caça: surfar altas explosivas de memes/lançamentos e encontrar projetos em consolidação antes do 100x.

## O que faz

### Radar
- Varre tokens recém-perfilados e em alta no **DexScreener** e pools novos/quentes por rede no **GeckoTerminal** (BSC, Base, Solana, Ethereum e mais).
- Filtra pela sua faixa (market cap, liquidez mínima, idade, rede) e calcula um **Gem Score** transparente com pesos ajustáveis (hype, momentum, liquidez, volume, pressão de compra, idade, lateralidade).
- Sinais: aceleração de volume (⚡ ignição), escada de momentum (5m/1h/6h/24h), FDV/diluição, liquidez para saída.
- Painel de **narrativas em alta** (IA, Meme, DeFi, RWA, DePIN, GameFi…) e categoria por token.
- Inspeção de contrato colado, exportação CSV, e link de segurança direto pro GMGN em cada card.

### Presets de estratégia
- **Padrão** — equilíbrio para descoberta geral.
- **Consolidação** — prioriza o sub-score **FLAT**: tokens em lateralidade (preço estável em 6h/24h) mas *vivos* (volume girando ≥5% da liquidez, 20+ compras/dia) e maduros (72h+). Detecta acumulação silenciosa aguardando explosão — token morto ou recém-nascido não engana o score.

### Segurança de contrato (GoPlus, automática a cada scan)
- **EVM** (ETH, BSC, Base, Arbitrum, Polygon, Avax): honeypot, criador com histórico de honeypot, taxa de venda ≥10%, mintável, pausável, blacklist, código fechado — ou 🛡 "GoPlus ok" com % de LP queimada/travada.
- **Solana**: token não-transferível, freeze authority (o "honeypot" da Solana), mint authority, saldo mutável.
- Cache de 12h por token. O veredito final continua sendo seu, no GMGN.

### Histórico e sinais temporais
- Cada scan grava snapshots por token (preço, mcap, liquidez, volume, buys) — sincronizados na nuvem (Supabase) e mesclados entre dispositivos.
- **📈 acumulando Xd** — preço lateral com liquidez subindo por 24h+ de observação (o padrão pré-explosão).
- **🚀 despertando** — volume 2×+ vs snapshot anterior em token já observado.
- **👁 N× scans** — sobrevivência entre scans (filtro natural contra rugs).

### Watchlist
- Salva com **preço de entrada**: o card mostra 💾 % desde o save, sincronizado entre dispositivos.

### 🧠 Smart Money
- **Extrator de compradores iniciais**: cole a CA de um token que explodiu e o app lê os primeiros Transfer logs direto da blockchain (RPC público, sem chave) — encontra sozinho o bloco onde o trading realmente começou, mesmo quando a liquidez entrou horas após a criação do par.
- Cada carteira vem classificada (`compra` = veio do pool, `mint` = dev/alocação, `transfer` = presale/insider) com minutos desde o lançamento e link direto pro perfil no GMGN.
- **Rastreamento com interseção**: carteiras presentes na janela inicial de 2+ tokens explodidos ganham ⭐ automaticamente — o sinal mais forte do app.
- **Cruzar com o radar**: compara os trades recentes dos top tokens do radar com suas carteiras rastreadas — acertos viram chip roxo 🧠 no card e entram no feed de compras.
- Suporte atual: chains EVM. (Solana no roadmap, via Helius.)

## Como rodar

É um único arquivo estático. Basta abrir `index.html` no navegador — ou hospedar (veja abaixo). Hospedado funciona melhor, porque as APIs públicas liberam as chamadas a partir de um site real.

### Nuvem (opcional, grátis)

Sem configuração, tudo funciona no `localStorage` do navegador. Para sincronizar histórico, watchlist e smart wallets entre dispositivos, crie um projeto gratuito no [Supabase](https://supabase.com), rode o SQL abaixo no SQL Editor e ajuste `SB_URL`/`SB_KEY` no `index.html`:

```sql
create table scans (
  key text not null, t bigint not null,
  p double precision, mc double precision, lq double precision,
  v double precision, b integer,
  primary key (key, t)
);
create table watchlist (
  key text primary key, sym text, chain text, addr text, url text,
  saved_at bigint, price_at double precision, mcap_at double precision
);
create table smart_wallets (
  addr text not null, chain text not null, source_token text not null,
  source_sym text, label text, added_at bigint,
  primary key (addr, chain, source_token)
);
alter table scans enable row level security;
alter table watchlist enable row level security;
alter table smart_wallets enable row level security;
create policy "anon_all_scans" on scans for all using (true) with check (true);
create policy "anon_all_watchlist" on watchlist for all using (true) with check (true);
create policy "anon_all_smart" on smart_wallets for all using (true) with check (true);
create index scans_t_idx on scans (t);
```

Snapshots com mais de 7 dias são podados automaticamente — o banco fica em poucos MB para sempre. A pill **☁ sync** no topo confirma a conexão. Nota: a anon key fica visível no HTML publicado; para um radar pessoal de dados públicos o risco é baixo, mas login com RLS por usuário está no roadmap.

## Fontes de dados (todas gratuitas, sem chave)

- [DexScreener API](https://docs.dexscreener.com) — descoberta, perfis, boosts.
- [GeckoTerminal API](https://www.geckoterminal.com/dex-api) — pools novos/trending, trades recentes.
- [GoPlus Security API](https://gopluslabs.io/token-security-api) — checagem de contrato EVM + Solana.
- RPC públicos ([Blast API](https://blastapi.io/public-api), [PublicNode](https://publicnode.com)) — Transfer logs para o Smart Money.
- [Supabase](https://supabase.com) (free tier, opcional) — sincronização na nuvem.

## Deploy (Netlify)

Site estático, sem etapa de build:

- Build command: *(vazio)*
- Publish directory: *(raiz)*

Conectado ao GitHub, cada push publica automaticamente.

## Roadmap

- Login/senha com RLS por usuário no Supabase
- Preset **Lançamento** (momentum 5m/1h para memes recém-nascidos)
- Smart Money na Solana (Helius free tier)
- Auto-refresh com diff entre scans

## Aviso

Ferramenta de análise e gestão de risco — **não é recomendação financeira**. Nenhum score prevê valorização, e a checagem de segurança do contrato é sua responsabilidade (os chips GoPlus ajudam, mas o veredito final é o GMGN e seu próprio julgamento). Boost = atenção paga, não orgânica. Decisões e capital são seus.
