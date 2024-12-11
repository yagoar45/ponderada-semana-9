# Ponderada da Semana 9 (Programação - Yago)

## O objetivo dessa atividade é testar a criação do ambiente como IaC - Infraestrutura como Código usando terraform.

* Iremos seguir o tutorial passado para criar o ambiente

## 1. Criação das chaves na AWS 

* Primeiro, entraremos na conta da AWS e usaremos o serviço de IAM para selecionar um usuário e criar a "secret acess key" e a "acess key" com a permissão de administrador

![alt text](image.png)

* Criaremos o arquivo ".env" para salvar esses dados

* Rodamos esse comando bash abaixo para salvar as variáveis de ambiente

```bash

Get-Content .env | ForEach-Object {
    $line = $_ -replace '\s+', ''
    if ($line -match '^(.*?)=(.*?)$') {
        [System.Environment]::SetEnvironmentVariable($matches[1], $matches[2], [System.EnvironmentVariableTarget]::Process)
    }
}

```

## 2. Criação do main.tf para definir a infraestrutura

```bash

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}

provider "aws" {
  region  = "us-west-2" 
}

resource "aws_instance" "app_server" {
  ami           = "ami-0044a0897b53acfb6"
  instance_type = "t2.micro"

  tags = {
    Name = "ExampleAppServerInstance"
  }
}
```

## 3. Inicialização

* Para iniciar o arquivo, usaremos o comando: **terraform initi**

* Após isso, teremos esse retorno:

![alt text](image-1.png)

## 4. Formatação

* Para formatação, usaremos o comando: **terraform fmt**

* Após isso, teremos esse retorno:

![alt text](image-2.png)

## 5. Criação da Infra 

* Aplicaremos a infra usando o comando: **terraform apply**

* Após isso, teremos essa saída:

![alt text](image-3.png)

* Ele irá perguntar se está tudo ok e colocando "yes" ele irá criar a infra: 

![alt text](image-4.png)


## 6. Ver o estado atual

* Rodando o comando **terraform show** ele irá mostrar o estado atual:

```bash
resource "aws_instance" "app_server" {
  ami                                  = "ami-xxxxxx"
  instance_type                        = "t2.micro"
  associate_public_ip_address          = true
  availability_zone                    = "us-west-2b"
  instance_state                       = "running"
  monitoring                           = false
  private_ip                           = "172.31.x.x"
  public_ip                            = "xx.xx.xx.xx"
  subnet_id                            = "subnet-xxxxxx"
  security_groups                      = ["default"]
  tags                                 = {
    "Name" = "ExampleAppServerInstance"
  }

  root_block_device {
    delete_on_termination = true
    device_name           = "/dev/xvda"
    volume_size           = 8
    volume_type           = "gp2"
  }
}

...
```

## 7. Listar recursos do projeto

Rodando o comando **terraform state list**, conseguiremos listar os recursos usados:

![alt text](image-5.png)
