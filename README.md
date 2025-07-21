# Infrastruturas-AWS

Projeto no âmbito da unidade curricular de Sistemas de Informação na Nuvem

Autor: Tomás Nave
Ano Letivo: 2024/2025
Curso: Engenharia Informática

## 📌 Objetivo Geral
Este projeto consistiu na execução de uma sequência de laboratórios práticos cujo objetivo principal foi a construção progressiva de infraestruturas na AWS, aplicando boas práticas de Cloud Computing e DevOps. Ao longo dos vários laboratórios, foi possível:
- Compreender os principais serviços de rede e segurança da AWS;
- Aplicar princípios de arquitetura segura e escalável na cloud;
- Automatizar configurações e gerir permissões de forma controlada;
- Implementar mecanismos de segmentação de rede (subnets públicas e privadas);
- Controlar acessos com IAM, Security Groups e Network ACLs;
- Realizar deploys de máquinas virtuais e configurar conectividade privada e pública com segurança.

## 🔧 Laboratório 1 – Gestão de Permissões IAM e VPC Inicial
O primeiro laboratório teve como foco principal a configuração de utilizadores, permissões e políticas de acesso com **IAM**, além da criação da primeira infraestrutura de rede com VPC e subnets.

### Principais tarefas:
- Criação de **User Groups** (Alunos e Professores) com políticas específicas e restritas por tags e regiões;
- Criação de **IAM Roles** para instâncias EC2 com permissões para interagir com S3, Lambda e Translate;
- Definição de um **utilizador IAM** limitado, com permissões apenas para criação e gestão de redes (VPC, Subnets, ACLs, SGs);
- onstrução de uma VPC simples com 3 subnets, Internet Gateway, Route Table, Security Groups e ACLs.

Este laboratório permitiu consolidar os fundamentos de gestão de acessos e construção básica de rede em AWS, com atenção especial à restrição de ações via tags e regiões, o que segue boas práticas de segurança na cloud.

## 🏗️ Laboratório 2 – Arquitetura com Bastion Host e Segmentação de Rede
Neste laboratório, o objetivo foi criar uma infraestrutura segura e segmentada, seguindo padrões reais de arquitetura cloud: subnets públicas e privadas, Bastion Host, NAT Gateway, e controle de acessos refinado.

### Componentes criados:
- **VPC personalizada** com CIDR 10.0.0.0/16;
- **Public Subnet** (10.0.1.0/24) para alojar o Bastion Host;
- **Private Subnet** (10.0.2.0/24) para alojar a base de dados (DB instance);
- **NAT Gateway** com **Elastic IP**, para permitir que a instância privada aceda à internet de forma segura;
- **Security Groups:**
  - **SG1:** acesso SSH (porta 22) apenas a partir de IPs autorizados para a Bastion Host;
  - **SG2:** acesso à base de dados (porta 3306) apenas a partir da Bastion Host;
- **Network ACLs** para reforço da segurança ao nível da subnet, limitando tráfego às portas essenciais;
- **EC2 Instances:**
  - **Bastion Host:** com IP público e acesso SSH;
  - **DB Instance:** na subnet privada, sem IP público, com acesso apenas interno via Bastion.

### Testes realizados:
- Acesso via SSH à Bastion Host;
- Acesso à instância privada a partir da Bastion (usando SSH em "2 saltos");
- Instalação do **MariaDB** na instância privada (comprovando acesso à internet via NAT Gateway);

Este laboratório foi essencial para praticar a configuração de ambientes seguros e isolados, com ligação controlada à internet e entre instâncias, cumprindo padrões reais de produção em Cloud.

## 🏗️ Laboratório 3 – Infraestrutura Web Completa com Duas VPCs, WordPress e Base de Dados
Neste laboratório, o objetivo foi criar uma infraestrutura completa e funcional na AWS para alojamento de uma aplicação WordPress com base de dados MySQL, aplicando boas práticas de segurança, segmentação de rede e acesso controlado através de Bastion Host e VPC Peering.

### Componentes criados:
- **Duas VPCs separadas** (pública e privada), ligadas entre si por VPC Peering;
- **Subnets:**
  - **VPC Pública:**
    - Subnet pública para Bastion Host e WordPress;
  - **VPC Privada:**
    - Subnet privada para a instância de base de dados;
- **Route Tables** configuradas para cada VPC, com regras específicas para Internet Gateway, NAT e peering;
- **NAT Gateway** na subnet pública da VPC pública, com Elastic IP atribuído, permitindo acesso à internet por parte da instância privada;
- **Security Groups:**
  - **SG1 (Bastion Host):** acesso SSH (porta 22) apenas a partir de IPs autorizados;
  - **SG2 (WordPress):** HTTP (80), HTTPS (443), SSH apenas a partir da Bastion Host;
  - **SG3 (DB Instance):** acesso MySQL (porta 3306) apenas a partir da instância WordPress;
- **Network ACLs** definidas para permitir apenas tráfego essencial nas subnets pública e privada;
- **Peering Connection** criada e configurada entre as duas VPCs, permitindo comunicação privada entre instâncias de redes distintas;
- **EC2 Instances:**
  - **Bastion Host:** instância t2.micro com IP público, utilizada como ponto de entrada SSH para o resto da infraestrutura;
  - **WordPress:** instância t2.micro com Nginx, PHP e WordPress instalados, acessível publicamente via HTTP/HTTPS, com acesso SSH via Bastion;
  - **DB Instance:** instância t2.micro com MariaDB instalada, colocada na subnet privada da VPC privada, sem IP público, acessível apenas a partir do WordPress;

### Configurações realizadas:
- Configuração do MariaDB, criação da base de dados wordpress e do utilizador wpuser;
- Instalação e configuração do Nginx, PHP e WordPress;
- Configuração do ficheiro wp-config.php com ligação à base de dados privada;
- Ajuste de permissões e configurações do servidor Nginx para alojar corretamente o WordPress;

### Testes realizados:
- Acesso via SSH à Bastion Host;
- Acesso à instância WordPress e DB através da Bastion Host (SSH encadeado);
- Instalação e configuração bem-sucedida do MariaDB na instância privada;
- Comunicação funcional entre o WordPress (subnet pública) e a base de dados (subnet privada);
- Acesso público ao site WordPress através de HTTP/HTTPS;
- Validação das regras de segurança com Network ACLs e Security Groups aplicadas corretamente;

Este laboratório consolidou os conhecimentos adquiridos, permitindo a construção de uma arquitetura realista e segura para aplicações web distribuídas em cloud, com segmentação de serviços, isolamento da base de dados e acesso controlado, conforme as boas práticas de segurança em ambientes AWS.
