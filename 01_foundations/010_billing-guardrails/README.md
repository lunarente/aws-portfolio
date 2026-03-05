# AWS Cost Guardrails — Monitoramento de Custos e Alertas de Faturamento

## Contexto

A governança de custos é uma prática essencial em ambientes cloud.  
Sem mecanismos de monitoramento proativo, aumentos inesperados de consumo podem gerar gastos não planejados.

Este projeto implementa um mecanismo básico de monitoramento de custos utilizando serviços nativos da AWS.

## Objetivo

Implementar alertas automatizados de faturamento para monitorar os gastos da conta AWS e notificar responsáveis quando determinados limites de custo forem atingidos.

## Arquitetura

AWS Billing Metrics → Amazon CloudWatch → Amazon SNS → Notificação por Email

O Amazon CloudWatch monitora a métrica `EstimatedCharges` e dispara alarmes quando os limites definidos são ultrapassados.  
As notificações são enviadas através do Amazon SNS.

## Serviços Utilizados

- Amazon CloudWatch
- Amazon SNS
- AWS Billing

## Implementação

### 1. Ativação das métricas de faturamento

Os alertas de faturamento foram habilitados no **AWS Billing Dashboard**, permitindo que o CloudWatch tenha acesso às métricas de custo da conta.

### 2. Canal de notificação

Foi criado um tópico SNS para envio das notificações de alerta.


Nome do tópico: 

````billing-alerts````

Uma assinatura por e-mail foi configurada para receber notificações sempre que um alarme for acionado.

### 3. Criação dos alarmes de faturamento

Foram configurados três alarmes no CloudWatch para monitorar a métrica `EstimatedCharges`.

| Alarme | Limite |
|------|------|
| billing-alarm-5usd | $5 |
| billing-alarm-25usd | $25 |
| billing-alarm-50usd | $50 |

Cada alarme envia uma notificação para o tópico SNS quando o limite definido é ultrapassado.

## Validação

Os alarmes podem ser verificados em:

```CloudWatch → Alarms```

Estado esperado:

```OK```

Se o valor monitorado ultrapassar o limite configurado:

```ALARM```

e uma notificação será enviada por e-mail.

## Considerações de custo

Esta configuração está incluída no **AWS Free Tier**.

Limite relevante:

- até **10 billing alarms gratuitos**

Este projeto utiliza 3 alarmes

# Evidências

Os prints da configuração estão disponíveis na pasta:

```evidence/```


## Limpeza do ambiente

Após a validação do funcionamento dos alertas, os seguintes recursos foram removidos para manter a conta organizada:

- alarmes do CloudWatch
- tópico SNS de notificação

## Principais aprendizados

- Implementação de monitoramento proativo de custos na AWS
- Integração entre CloudWatch e SNS
- Boas práticas iniciais de governança financeira em ambientes cloud

## Conhecimento para entrevistas

**Onde as métricas de faturamento são habilitadas na AWS?**
R. Billing → Billing Preferences → Enable Billing Alerts

**Qual serviço envia a notificação dos alarmes?**
R. Amazon SNS

**Qual métrica é utilizada para monitorar custos da AWS?**
R. EstimatedCharges

## Possíveis melhorias em ambiente de produção

- Integração de alertas com Slack ou Microsoft Teams
- Implementação de AWS Budgets para controle mensal
- Monitoramento centralizado de custos utilizando AWS Organizations

## Referências

[Crie um orçamemento](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-create.html#create-cost-budget)
[Melhores práticas](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-best-practices.html)

## Saída esperada 

[texto alternativo](01_budget-created.png)


