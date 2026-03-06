# AWS EC2 — Deploy de Website "Hello World"

# Contexto

Serviços de computação são um dos pilares fundamentais da computação em nuvem.  
O Amazon EC2 permite executar servidores virtuais sob demanda, possibilitando o deploy de aplicações web de forma escalável e controlada.

Neste projeto foi realizado o provisionamento de uma instância EC2 em uma subnet pública, configurando regras de segurança adequadas e instalando um servidor web para disponibilizar um site simples acessível pela internet.

---

# Objetivo do projeto

Implementar um servidor web simples utilizando **Amazon EC2**, permitindo acesso público através de um navegador.

O projeto inclui:

- criação de uma instância Linux EC2
- configuração de regras de segurança
- acesso via SSH
- instalação de servidor web
- publicação de uma página HTML simples

---

# Arquitetura

Internet → Security Group → EC2 Instance → Web Server (Apache)

A instância EC2 é executada em uma **subnet pública**, permitindo acesso externo através do IP público.

---

# Implementação

## 1. Criar uma instância EC2

No console AWS:

```EC2 → Instances → Launch Instances```


Configuração utilizada:

```Name and tags: (nome)```

Sistema operacional:


```Amazon Linux 2023 (ou Ubuntu)```


Tipo de instância:

```t3.micro```

Key pair (login) 

```Create key pair - name - RSA - .PEM```

Região: qualquer região disponível.

---

## 2. Configurar Security Group

Regras de entrada configuradas:

| Type | Port | Source    |
| ---- | ---- | --------- |
| SSH  | 22   | My IP     |
| HTTP | 80   | 0.0.0.0/0 |

---

## 3. Conectar à instância EC2 via SSH

Utilizando a chave `.pem` associada à instância:

```ssh -i chave.pem ec2-user@<IP_PUBLICO>```

---

## 4. Instalar servidor web

Instalar Apache:

```sudo yum install httpd -y```
```sudo systemctl start httpd```
```sudo systemctl enable httpd```

---

## 5. Criar página Hello World

Criar arquivo HTML:

```sudo nano /var/www/html/index.html```

---

Conteúdo exemplo:

```<h1>Hello World from AWS EC2</h1>```

---

## 6. Testar acesso

Abrir navegador e acessar:

```http://IP_PUBLICO_DA_INSTANCIA```


A página Hello World deverá ser exibida.

---

# Perguntas técnicas

### 1. O que é uma região na AWS?

Uma região é uma localização geográfica onde a AWS possui infraestrutura de datacenters.
Cada região contém múltiplas **Availability Zones**, projetadas para fornecer alta disponibilidade e tolerância a falhas.

Exemplos de regiões:

`us-east-1` (North Virginia)
`eu-west-1` (Irlanda)
`sa-east-1` (São Paulo)

---

### 2. O que é uma Availability Zone?

Uma **Availability Zone (AZ)** é um datacenter isolado dentro de uma região AWS.

Cada região possui múltiplas AZs conectadas por rede de baixa latência, permitindo que aplicações distribuídas entre zonas tenham maior resiliência.

Exemplo:

```
us-east-1a
us-east-1b
```

---

### 3. O que é uma subnet pública?

Uma subnet pública é uma subnet dentro de uma **VPC** que possui rota para um **Internet Gateway (IGW)**.

Instâncias dentro dessa subnet podem receber tráfego diretamente da internet quando possuem IP público.

---

### 4. Quantas subnets podem existir em uma região?

O número de subnets depende da configuração da VPC.

Por padrão:

```
até 200 subnets por VPC
```

Esse limite pode ser aumentado mediante solicitação à AWS.

---

### 5. Como lançar instâncias em subnets públicas ou privadas?

**Subnet pública**

* selecionar subnet pública
* habilitar IP público automático
* configurar Security Group permitindo acesso externo

**Subnet privada**

* selecionar subnet privada
* desabilitar IP público
* utilizar NAT Gateway ou bastion host para acesso externo

---

### 6. O que são AMIs?

**Amazon Machine Images (AMI)** são templates de máquinas virtuais usados para criar instâncias EC2.

Elas contêm:

* sistema operacional
* configurações básicas
* softwares pré-instalados

---

### 7. O que são Security Groups?

Security Groups são **firewalls virtuais** que controlam o tráfego de entrada e saída das instâncias EC2.

Eles permitem definir regras específicas de acesso baseadas em:

* portas
* protocolos
* endereços IP

---

### 8. O que são regras inbound e outbound?

**Inbound rules**

Controlam o tráfego que entra na instância.

Exemplo:

```
SSH porta 22
HTTP porta 80
```

**Outbound rules**

Controlam o tráfego que sai da instância.

Por padrão, todo tráfego de saída é permitido.

---

### 9. O que significa "deny by default"?

Security Groups seguem o princípio de **negação por padrão**.

Isso significa que **todo tráfego é bloqueado**, exceto aquele explicitamente permitido pelas regras configuradas.

---

### 10. Como permitir acesso a uma instância EC2?

Editando as **regras inbound do Security Group**.

Exemplo:

permitir SSH porta 22 do seu IP
permitir HTTP porta 80 para `0.0.0.0/0`

---

### 11. Como conectar na instância EC2?

Utilizando SSH e a chave privada associada à instância:

```ssh -i chave.pem ec2-user@IP_PUBLICO```

---

### 12. Como hospedar um site simples em EC2?

1. Criar instância EC2
2. Configurar Security Group para HTTP
3. Conectar via SSH
4. Instalar Apache ou Nginx
5. Criar arquivos HTML em

```/var/www/html```

6. Acessar via navegador utilizando o IP público.

---

# Referências


* [Documentação oficial AWS](https://aws.amazon.com/ec2/)


* [Security Groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html)


* [Instalação Apache](http://httpd.apache.org/docs/2.4/install.html)


* [Acesso SSHl](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)

---

# Custos

Este projeto pode ser executado utilizando o **AWS Free Tier**.

Instância recomendada:

```t3.micro```

---

# Aprendizados

* provisionamento de instâncias EC2
* configuração de Security Groups
* acesso remoto via SSH
* instalação de servidor web
* deploy de site estático em infraestrutura cloud

---

# Bullet para currículo

Provisionamento de infraestrutura web em AWS utilizando **Amazon EC2**, incluindo configuração de rede, Security Groups, acesso SSH e deploy de servidor Apache para hospedagem de aplicação web estática.

---

# Saída Esperada

![Hello World EC2](evidence\03_hw-ec2.png)
    
