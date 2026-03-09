# Amazon ECR — Publicação de Imagem Docker em Registry Privado


# Contexto

Em arquiteturas modernas baseadas em containers, é necessário armazenar, versionar e distribuir imagens de forma segura e padronizada.

O **Amazon Elastic Container Registry (ECR)** é o serviço gerenciado da AWS para armazenamento de imagens Docker, sendo amplamente utilizado como base para workloads em **ECS, EKS, Lambda com containers e pipelines de dados containerizados**.

Neste projeto foi realizado o envio de uma imagem Docker para um repositório privado no Amazon ECR, incluindo autenticação via AWS CLI, build local da imagem, tagueamento e publicação no registry.

---

# Objetivo do projeto

Implementar o processo de publicação de uma imagem Docker em um repositório privado no Amazon ECR.

O projeto inclui:

- criação de repositório no Amazon ECR
- autenticação do Docker no registry da AWS
- criação de imagem Docker local
- associação de tag compatível com o ECR
- push da imagem para o repositório remoto

---

# Arquitetura

Ambiente local → Docker → Amazon ECR

A imagem é construída localmente, autenticada no registry da AWS e enviada para um repositório privado no Amazon ECR.

---

# Implementação

## 1. Criar repositório no Amazon ECR

No console AWS:

Amazon ECR → Create repository

Configuração utilizada:

- tipo: `Private`
- nome do repositório: `data-engineering-labs`
- tag immutability: `Disabled`
- scan on push: `Enabled`

Após a criação, o repositório gera uma URL no formato:

```

ACCOUNT_ID.dkr.ecr.REGION.amazonaws.com/REPOSITORY

```

Exemplo:

```

123456789012.dkr.ecr.us-east-1.amazonaws.com/data-engineering-labs

````

---

## 2. Autenticar o Docker no ECR

No terminal:

```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com
````

Resultado esperado:

```
Login Succeeded
```

---

## 3. Criar a imagem Docker

Criar um arquivo chamado **Dockerfile**.

Conteúdo:

```dockerfile
FROM python:3.10

WORKDIR /app

COPY . .

CMD ["python", "-c", "print('AWS Data Engineering Lab Container')"]
```

Build da imagem:

```bash
docker build -t data-lab-image .
```

Verificar imagens locais:

```bash
docker images
```

---

## 4. Aplicar tag compatível com o ECR

Para enviar ao registry da AWS, a imagem precisa receber a tag do repositório ECR:

```bash
docker tag data-lab-image:latest 123456789012.dkr.ecr.us-east-1.amazonaws.com/data-engineering-labs:latest
```

---

## 5. Publicar imagem no Amazon ECR

Executar o push da imagem:

```bash
docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/data-engineering-labs:latest
```

Resultado esperado:

```
latest: digest: sha256:...
```

---

# Validação

A validação pode ser realizada em:

```
AWS Console → Amazon ECR → data-engineering-labs
```

Resultado esperado:

* imagem publicada com tag `latest`
* repositório visível no console
* scan de vulnerabilidades habilitado

---

# Perguntas técnicas

## 1. Qual é o formato da URL de um registry no Amazon ECR?

O formato é:

```
ACCOUNT_ID.dkr.ecr.REGION.amazonaws.com/REPOSITORY
```

Exemplo:

```
123456789012.dkr.ecr.us-east-1.amazonaws.com/data-engineering-labs
```

---

## 2. O que é tag immutability?

Tag immutability impede que uma tag existente seja sobrescrita.

Esse recurso é importante para:

* rastreabilidade
* reprodutibilidade
* controle de versões de deploy

---

## 3. O que significa scan on push?

`Scan on push` faz a análise automática da imagem enviada ao repositório para identificar vulnerabilidades conhecidas.

Esse processo contribui para a segurança da cadeia de entrega de software.

---

## 4. Como ocorre a autenticação no Amazon ECR?

A autenticação pode ser feita por meio de um **token temporário obtido via AWS CLI**, utilizando credenciais IAM autorizadas para acesso ao ECR.

Também é comum utilizar:

* IAM Roles
* políticas IAM
* autenticação integrada com ECS ou EKS

---

## 5. Qual é a validade do token de autenticação do ECR?

O token de autenticação do Amazon ECR possui validade de:

```
12 horas
```

---

## 6. Por que o Amazon ECR é importante em arquiteturas modernas?

Porque ele centraliza o armazenamento e o versionamento de imagens de container, permitindo integração com pipelines de CI/CD e serviços gerenciados da AWS.

É amplamente utilizado em workloads baseados em:

* ECS
* EKS
* Lambda com containers
* pipelines de dados containerizados
* aplicações de MLOps

---

# Custos

Este projeto pode ser executado com custo muito baixo ou dentro do uso de laboratório.

Como boa prática, os recursos devem ser removidos após a validação.

---

# Limpeza do ambiente

Após concluir o teste, os recursos podem ser removidos.

## Remover imagem do repositório

AWS Console → Amazon ECR → Repository → Delete image

## Remover repositório

AWS Console → Amazon ECR → Delete repository

## Remover imagem local

```bash
docker rmi data-lab-image
```

Se necessário remover também a imagem com a tag do ECR:

```bash
docker rmi 123456789012.dkr.ecr.us-east-1.amazonaws.com/data-engineering-labs:latest
```

---

# Evidências

Os prints da implementação devem ser armazenados na pasta:

```
evidence/
```

Exemplos:

```
01-ecr-repository-created.png
02-docker-login-success.png
03-docker-build.png
04-image-pushed.png
```

---

# Aprendizados

* criação de repositório privado no Amazon ECR
* autenticação segura entre Docker e AWS
* publicação e versionamento de imagens Docker
* uso de registry gerenciado como base para arquiteturas containerizadas
* aplicação de práticas iniciais de segurança com scan de vulnerabilidades

---

# Bullet para currículo

Implementação de pipeline básico de publicação de containers utilizando **Amazon ECR e Docker**, incluindo autenticação segura via AWS CLI, build de imagem, versionamento e push para registry privado com verificação automática de vulnerabilidades.

````

---


