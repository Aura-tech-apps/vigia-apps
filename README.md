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

| Arquivo | Quando | O que olha | Quando avisa |
|---|---|---|---|
| `monitor.yml` | De 10 em 10 min | O app Energetic Seed responde? (3 tentativas, conferindo o conteúdo) | Não respondeu nenhuma vez |
| `monitor-aurachat.yml` | De 10 em 10 min | O Aura Chat consegue **gravar** no banco? (sonda profunda) | Não respondeu nenhuma vez |
| `monitor-sara.yml` | De 10 em 10 min | A Sara do WhatsApp responde, com banco e Redis de pé? | Não respondeu nenhuma vez |
| `certificado.yml` | 1x por dia | Validade do certificado direto na VPS | Falta menos de 21 dias |
| `certificado.yml` | 1x por dia | O caminho de renovação do certificado está aberto? | Respondeu algo fora do esperado |

**Um arquivo por aplicativo, de propósito.** O freio de e-mail (abaixo) conta
quantas execuções seguidas do MESMO arquivo falharam. Se dois aplicativos
dividissem um arquivo, a queda de um contaminaria a contagem do outro e o
primeiro aviso de uma queda nova poderia ser engolido como se fosse continuação
da anterior.

**Cada um bate num endereço com `/api` na frente.** Sem isso, o servidor entrega
a página do painel — 200 com HTML — e o vigia fica verde para sempre com o
aplicativo morto.

## O freio de e-mail

Com checagem de 10 em 10 minutos, um app fora do ar por um dia mandaria **144
e-mails**. Além de inútil, volume assim queima a reputação do domínio no Resend e
derruba a entrega dos e-mails que vão pras clientes.

Então: **primeiro aviso na hora, e depois no máximo um por hora** enquanto
continuar fora do ar. O vigia descobre há quanto tempo está caído perguntando ao
próprio GitHub quantas execuções seguidas já falharam — não guarda estado em
lugar nenhum.

Por isso a execução fica **vermelha** sempre que o app está fora, mesmo quando o
e-mail foi segurado: é essa cor que alimenta a contagem da próxima vez.

## Segredos necessários

- `RESEND_API_KEY` — para enviar o e-mail de aviso
- `VPS_ORIGEM` — endereço direto do servidor, para conferir o certificado
