# 🚀 Laboratório: Explorando a AWS com Amazon EC2

![1](https://raw.githubusercontent.com/HalleyVeras/aws-ec2-lab-developer-EDN/refs/heads/main/arquivos/Leonardo_Phoenix_10_Crie_um_banner_em_estilo_3D_cartoon_com_es_2.jpg)

> Curso: Developer | Escola da Nuvem  
> Autor: Halley Veras  
> Repositório dedicado à prática com EC2 via Console e CloudShell.

---

## 📌 Objetivo

Aprender a:

- Inicializar e configurar uma instância EC2 via Console AWS.
- Criar servidor web de teste acessível por HTTP.
- Executar uma nova instância via **AWS CloudShell** usando comandos no terminal.
- Finalizar instâncias EC2 corretamente.

---

## ✅ Pré-requisitos

- Conta AWS fornecida pela Escola da Nuvem.
- Acesso ao Console da AWS e CloudShell.
- Navegador de internet.
- Atenção aos **custos**: utilize apenas os recursos permitidos no lab.

---

## 🖥️ Parte 1: Criando uma Instância EC2 via Console

### 1. Acesse o Console AWS
👉 [console.aws.amazon.com](https://console.aws.amazon.com)

> ⚠️ Verifique se está logado com a conta correta (canto superior direito do console).

---

### 2. Pesquise por `EC2` e clique em **Executar Instância**

🔧 Configure os seguintes parâmetros:

| Campo                        | Valor sugerido                      |
|-----------------------------|-------------------------------------|
| Nome                        | `webserver-halleyveras`            |
| AMI                         | Amazon Linux 2                      |
| Tipo de instância           | `t2.micro`                          |
| Par de chaves               | `parchave-halleyveras` (.pem)      |
| Grupo de segurança          | Criar novo - permitir tráfego HTTP |
| Dados de usuário (User Data)| *(veja abaixo)*                     |

📜 **Script de inicialização:**
```bash
#!/bin/bash
yum -y install httpd
systemctl enable httpd
systemctl start httpd
echo '<html><h1>Olá do seu servidor web!</h1></html>' > /var/www/html/index.html
```

---

### 3. Acesse seu servidor via navegador

📍 Após a instância estar em execução e com o status **2/2 checks passed**:

🔗 Acesse: `http://[seu-endereço-ip-público]`

🖼️ *Adicione aqui o print do navegador com a página “Olá do seu servidor web!”*

---

## 💻 Parte 2: Criando Instância via AWS CloudShell

### 1. Acesse o CloudShell

🔍 No canto superior direito do console, clique no ícone do terminal (`>_`)

---

### 2. Execute os comandos abaixo (personalize com seu nome)

```bash
GRUPO_SEGURANCA="halleyveras-grupo"
NOME_INSTANCIA="instancia-halleyveras"
PAR_CHAVE="parchave-halleyveras"
```

```bash
aws ec2 create-security-group --group-name $GRUPO_SEGURANCA \
 --description "Grupo de segurança para EC2 via CloudShell"
```

```bash
aws ec2 authorize-security-group-ingress --group-name $GRUPO_SEGURANCA \
 --protocol tcp --port 80 --cidr 0.0.0.0/0
```

---

### 3. Inicialize a nova instância via CLI

```bash
aws ec2 run-instances \
 --image-id ami-0c55b159cbfafe1f0 \
 --count 1 \
 --instance-type t2.micro \
 --key-name $PAR_CHAVE \
 --security-groups $GRUPO_SEGURANCA \
 --user-data '#!/bin/bash
yum -y install httpd
systemctl enable httpd
systemctl start httpd
echo "<html><h1>Servidor iniciado via CloudShell</h1></html>" > /var/www/html/index.html'
```

📌 Aguarde alguns minutos e obtenha o IP com:

```bash
aws ec2 describe-instances --query "Reservations[*].Instances[*].PublicIpAddress" --output text
```

🖼️ *Adicione aqui o print da página web iniciada via CloudShell*

---

## 🧹 Finalizando Instâncias

> Finalize as instâncias após o uso para evitar custos desnecessários.

Via Console:  
☑️ Acesse EC2 > Instâncias > Ações > Estado da Instância > Encerrar

Via CLI:  
🔻 Identifique o ID da instância e execute:
```bash
aws ec2 terminate-instances --instance-ids i-xxxxxxxxxxxxxxxxx
```

---

## 📸 Galeria de Capturas

> Adicione abaixo os prints que demonstram suas etapas no laboratório:

- ✅ Instância criada via Console
- 🌐 Página "Olá do seu servidor web!"
- 🖥️ CloudShell com comandos executados
- 🔗 Página iniciada via CloudShell

---

## 💡 Dica Extra

Você pode automatizar a criação de múltiplas instâncias com scripts Shell e integrar com **CloudFormation** em labs futuros!

---

## 📁 Estrutura de Diretórios (opcional)

```bash
📦aws-ec2-lab
 ┣ 📂prints/
 ┃ ┣ 📷console-instance.png
 ┃ ┣ 📷cloudshell-instance.png
 ┣ 📄README.md
 ┗ 📄scripts.sh
```

---

## 🧠 Conclusão

Esse laboratório oferece uma introdução prática ao poder e flexibilidade do Amazon EC2. Ao dominar tanto o Console quanto o CloudShell, você está um passo mais próximo de construir arquiteturas escaláveis e automatizadas na AWS!

---

> ✨ Projeto feito com dedicação por [Halley Veras](https://github.com/halleyveras) durante o curso Developer da [Escola da Nuvem](https://escoladanuvem.org).
