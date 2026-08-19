# central-automacoes-releases

Canal público de atualização da **Central de Automações** (Solution Facilities).

Este repositório contém **apenas artefatos de release**. Nenhum código-fonte
vive aqui — o repositório da aplicação é privado.

## Conteúdo

| Arquivo | O que é |
|---|---|
| `manifest.json` | versão corrente, URL do executável, `sha256`, `min_versao_suportada`, notas |
| `manifest.sig` | assinatura **Ed25519** do manifesto |
| releases (tags `vX.Y.Z`) | o `CentralAutomacoes_v1.exe` de cada versão |

## Como as instalações usam isto

O cliente embarcado (`util/atualizador.py`) lê `manifest.json` a cada 6h,
**valida hash SHA-256 e assinatura Ed25519** contra a chave pública que viaja
dentro do próprio executável, e só então baixa. A troca é feita por
`tools/aplicar_atualizacao.py`, com **rollback automático** por health-check em
`/api/version`.

Um manifesto sem assinatura válida é descartado sem download. A chave privada
nunca esteve neste repositório nem no repositório da aplicação.

## Se algo der errado

Reverter não é apagar o release ruim — máquinas que já baixaram podem não ter
mais o `.backup.exe`. Publica-se um manifesto **novo** apontando para a versão
anterior, subindo `min_versao_suportada` para forçar a volta.
