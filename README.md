# port-store-data

Repositório de **dados dos jogos** do Port Store (o segundo tipo de
repositório do app — o primeiro é o repositório de APPS da Loja).

Vem desativado no app e é ativado com a **senha do repositório** em
Configurações do Launcher.

## Formato (v2 — links)

O repositório NÃO hospeda mais os pacotes: `index.json` aponta para
**links externos** de download. O app resolve o link direto do provedor
(ex.: página do Buzzheavier) e baixa o pacote `tar.gz` no formato de
backup v3 do Port Store (manifest.json + install/apk + install/extdata).

```
index.json            índice de todos os jogos (formato port-store-data v2)
```

### Estrutura de `index.json`

```json
{
  "format": "port-store-data",
  "version": 2,
  "passwordCheck": { "kdf": "PBKDF2WithHmacSHA256", "salt": "...", "iterations": 180000, "verifier": "..." },
  "games": [
    {
      "gameId": "pt-android",
      "packageName": "com.ptport.app",
      "gameName": "P.T.",
      "appVersion": "v0.2.0",
      "manifest": { "formatVersion": 3, "...": "manifest v3 embutido" },
      "source": "buzzheavier",
      "url": "https://buzzheavier.com/<id>",
      "compression": "gzip"
    }
  ]
}
```

- `passwordCheck`: verificador PBKDF2-HMAC-SHA256 da senha (ativação local).
- `games[].url`: PÁGINA do provedor — o app extrai o link direto em Rust
  (motor nativo) antes de baixar.
- `games[].manifest`: manifest v3 embutido (mesmo formato do exportar/
  importar) — usado para progresso da extração e checagem de espaço.

## Histórico

- **v1** (removido): pacotes criptografados AES-256-CTR em partes de
  95 MiB hospedadas aqui (`.psenc.partNN`). Mantido no histórico do git.
