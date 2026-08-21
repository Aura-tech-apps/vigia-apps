# vigia-apps

Robô que olha os aplicativos **de fora** e avisa por e-mail quando algum sai do ar.

## Por que este repositório é público

O vigia precisa viver **fora** do servidor que ele vigia. Se ele morasse na VPS,
morreria junto com ela — justamente no momento em que o aviso mais importa.

Ele morava no repositório privado do aplicativo. Em 21/08/2026 a organização
estourou a cota mensal de execuções (2.000 minutos) e **o vigia parou junto com
todo o resto**. Repositório público tem execuções ilimitadas, então aqui ele
nunca mais fica sem saldo.

## O que tem aqui (e o que não tem)

Só o robô. Nenhum código de aplicativo, nenhuma senha, nenhum dado de cliente.
Ele apenas consulta endereços que já são públicos e, se algo estiver errado,
manda um e-mail.

O endereço direto do servidor **não** aparece nos arquivos: vive num segredo
(`VPS_ORIGEM`), porque publicá-lo entregaria a máquina que a Cloudflare existe
para esconder.

## O que ele confere

| Quando | O que olha | Quando avisa |
|---|---|---|
| De 2 em 2 horas | O app responde? (3 tentativas, conferindo o conteúdo) | Não respondeu nenhuma vez |
| 1x por dia, de manhã | Validade do certificado direto na VPS | Falta menos de 21 dias |
| 1x por dia, de manhã | O caminho de renovação do certificado está aberto? | Respondeu algo fora do esperado |

## Segredos necessários

- `RESEND_API_KEY` — para enviar o e-mail de aviso
- `VPS_ORIGEM` — endereço direto do servidor, para conferir o certificado
