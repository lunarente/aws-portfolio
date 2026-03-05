# Billing Guardrails — Budgets & Alerts

## Contexto
Em ambientes corporativos, controle de custos precisa ser proativo: alertas e orçamento evitam surpresas e ajudam a manter governança.

## Objetivo
- Criar um orçamento mensal de custos
- Configurar alertas por limite (ex.: 50%, 80%, 100%)
- Registrar evidências e aprendizado

## Arquitetura (alto nível)
AWS Budgets -> Notificações (email / SNS)

## Implementação (resumo)
1. Habilitei acesso ao Billing para usuários IAM (quando aplicável)
2. Criei um budget mensal de custo total
3. Configurei alertas em thresholds progressivos
4. Registrei evidências (prints/config) em `evidence/`

## Evidências
Veja `evidence/`.

## Custos e segurança
- Orçamento ajuda a controlar gastos e reforça governança
- Evitei permissões amplas e documentei pré-requisitos de acesso

## Q&A (para entrevista)
**Quais permissões IAM são necessárias para criar Budgets?**  
Depende do modelo de conta. Em geral, além das permissões do serviço de Budgets, o usuário precisa ter acesso ao console de Billing (habilitado no nível da conta) para visualizar/gerenciar Billing & Cost Management.

## Aprendizados
- Diferença entre alarmes do CloudWatch Billing e Budgets
- Como estruturar alertas progressivos para reduzir risco
- Como evidenciar implementação para auditoria