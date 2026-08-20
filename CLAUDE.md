# 📘 CONTEXTO GERAL DO PROJETO & ARQUITETURA — PAINEL ADS & OS 4 PLANOS PABLO WANDERSON

**Para a IA:** Você está atuando no ecossistema completo de tecnologia, automação e inteligência de negócios do criador e empreendedor **Pablo Wanderson**. Este ecossistema é composto pelo **Painel Ads (E-commerce + Meta/Google Ads)** e pelos **4 Planos de Automação com IA**. Leia atentamente as diretrizes e aprendizados consolidados abaixo antes de propor qualquer alteração.

---

## 🏛️ ESTRUTURA GERAL DO ECOSSISTEMA

```text
sistema/
├── app/                        # Next.js App Router (E-commerce, Checkout EFI Bank, Páginas Ponte)
├── components/                 # Componentes React de Alta Conversão (TailwindCSS, Glassmorphism)
├── lib/                        # Integrações (Supabase, EFI Bank PIX mTLS, Cloudflare R2)
├── plano_3_redes_proprias/     # [PLANO 3] Automação das Redes Sociais Próprias (@eusoupablowanderson)
│   ├── src/
│   │   ├── ai/                 # Groq Llama 3.3 70B, Whisper Large v3, Trending Music Downloader
│   │   ├── bot/                # Telegram Bot (@Iapablo_bot) com Telegraf & GramJS MTProto
│   │   ├── publishers/         # Meta Graph v22.0, YouTube Data v3, TikTok Open API v2
│   │   └── video/              # FFmpeg, Hormozi Subtitles, Long Video Splitter, Silence Remover
│   └── assets/                 # Brand PW, Músicas Virais (-14 LUFS), Fontes
├── bot_social_poster/          # Documentação e módulos dos Planos 1 e 2
├── STATUS.md                   # Status consolidado e auditoria de todos os subsistemas
└── CLAUDE.md                   # Guia geral de comandos, arquitetura e aprendizados técnicos
```

---

## 🚀 OS 4 PLANOS DE AUTOMAÇÃO

1. **🟢 PLANO 1 — Robô de Produção Massiva de Vídeos Dark & Cortes Afiliados:**
   - *Status:* 100% Concluído & Operacional.
2. **🟢 PLANO 2 — Central de Inteligência de Tráfego Pago & Meta Ads:**
   - *Status:* 100% Concluído & Operacional (E-commerce próprio, Checkout EFI Bank, Google Merchant e Meta CAPI).
3. **🟢 PLANO 3 — Automação 100% Autônoma das Redes Sociais Próprias do Pablo Wanderson:**
   - *Status:* 100% Concluído, Auditado (10/10) e Ativo no Railway (`bot-pablo-wanderson-production.up.railway.app`).
   - Perfil oficial `@eusoupablowanderson` conectado via **Token Permanente (`expires_at: 0`)**.
4. **🟡 PLANO 4 — Portal & Canal de Notícias Regionais e Municipais com IA:**
   - *Status:* Arquitetura e especificações 100% desenhadas em `plano_4_noticias_locais/CLAUDE.md`.

---

## 🧠 APRENDIZADOS TÉCNICOS & DIRETRIZES DE ARQUITETURA CONSOLIDADAS

### 1. 🔐 Meta Graph API & Instagram Tokens Perpétuos
- **User Tokens vs Page Access Tokens:** User Tokens estendidos duram no máximo 60 dias. Contudo, **Page Access Tokens** obtidos via `GET /v22.0/me/accounts` a partir de uma Página vinculada a uma Conta Profissional do Instagram retornam `type: PAGE` e **`expires_at: 0` (NUNCA EXPIRAM)**.
- **Publicação de Fotos (Feed & Stories):** A API do Instagram v22.0 exige obrigatoriamente uma URL pública HTTPS (`image_url`). Os arquivos devem ser servidos via CDN pública do Express (`/public/temp/` ou `/public/processed/`).

### 2. 🎬 Motor FFmpeg, Mixagem de Áudio & Legendas
- **Divisão de Pads de Áudio (`asplit=2`):** Em filtros complexos com *Sidechain Compression* (Audio Ducking), o canal `0:a` da voz nunca pode ser referenciado duas vezes sem divisão prévia (`[0:a]asplit=2[a1][a2]`), caso contrário o FFmpeg retorna `Error initializing complex filters: Invalid argument`.
- **Prevenção de Congelamento de Imagem:** Para que a imagem do vídeo nunca trave no final quando a trilha de música for mais longa que o vídeo, utilize sempre `-t ${videoDuration}` nas opções de saída e aplique corte com *fade-out suave* (`afade=t=out:st=${dur-1.5}:d=1.5`).
- **Filtro de Legendas Condicional:** O filtro `subtitles='...'` ou `ass='...'` só deve ser adicionado ao `-filter_complex` se o arquivo `.ass` contiver linhas `Dialogue:` reais. Em vídeos de corrida/ambiente sem fala, o filtro de legendas deve ser omitido para evitar falha no parser do FFmpeg.
- **Sincronização Hormozi Sem Gaps:** O timestamp final de cada palavra (`endTime`) deve encostar exatamente no início da próxima (`startTime`), eliminando piscadas e atrasos em relação à voz.

### 3. 🌐 Músicas Virais em Tempo Real (Deezer Charts API)
- A API pública da Deezer (`https://api.deezer.com/search?q=...` e `/chart/73/tracks`) permite buscar e baixar trechos oficiais de 30-60s em MP3 de 320kbps em menos de 1 segundo sem necessidade de chave paga.
- As faixas de música de fundo devem ser masterizadas a **-14 LUFS** (`mean_volume: -12dB a -15dB`, `max_volume: 0dB`) para som potente e nítido no celular.

### 4. 🐧 Compatibilidade de Caminhos Multi-Plataforma (Windows ⇄ Railway Linux)
- Caminhos absolutos salvos no banco SQLite durante execuções locais no Windows (`C:\Users\...`) quebram ao rodar no container Linux do Railway (`/app/...`).
- Utilize sempre a função `resolveExistingPath(dbPath, defaultFolder)` para buscar pelo `path.basename()` na pasta correspondente do ambiente atual.

### 5. 💳 Checkout EFI Bank & mTLS
- O certificado de segurança da EFI deve ser injetado via variável de ambiente Base64 (`EFI_CERT_BASE64`) para evitar dependência de arquivos físicos locais nos containers do Railway.

---

## ⚡ COMANDOS ESSENCIAIS

```bash
# Push do E-commerce / Painel Ads:
git push origin main

# Push do Bot Pablo Wanderson (Plano 3):
cd plano_3_redes_proprias && git push origin main
```
