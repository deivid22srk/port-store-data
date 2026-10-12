# port-store-data

Repositório de **dados complementares dos jogos** do Port Store — o segundo
tipo de repositório do ecossistema (o primeiro é o repositório de apps,
consumido pela aba Loja).

> **Ativação no app:** o repositório vem pré-configurado no Port Store, porém
> **desativado**. Para ativar, informe a **senha do repositório** em
> Configurações → Repositório de dados de jogos. A senha é verificada localmente
> (PBKDF2) e é necessária para decriptar os pacotes baixados.

## Formato

```
index.json                                     índice de todos os jogos
games/<gameId>/manifest.json                   manifest v3 (legível, sem senha)
games/<gameId>/<prefixo>_portstore.tar.gz.psenc.part00..N   partes criptografadas
```

- Cada pacote é um `.tar.gz` **v3 do Port Store** (o mesmo formato do
  exportar/importar do Modo Desenvolvedor), **criptografado** com
  AES-256-CTR (chave derivada da senha do repositório via PBKDF2-HMAC-SHA256,
  180.000 iterações).
- As partes têm no máximo 95 MiB (limite de arquivo do GitHub). O app baixa
  as partes em sequência, valida o SHA-256 de cada uma, decripta em streaming
  (sem arquivo intermediário) e extrai direto no ambiente virtual.
- O `index.json` contém o verificador da senha (PBKDF2), os metadados de cada
  jogo (manifest embutido) e a lista de partes com SHA-256 individual.

## Adicionar/atualizar um jogo

Gere um pacote pelo próprio app (Modo Desenvolvedor → Exportar dados do jogo),
envie o `.tar.gz` para o mantenedor, que roda o script de empacotamento
(criptografa + divide + atualiza o `index.json`) e publica aqui.
A senha pode ser trocada regenerando o `index.json` e as partes — apps antigos
com a senha velha deixam de decriptar.
