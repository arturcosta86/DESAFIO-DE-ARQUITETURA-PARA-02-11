# Desafio de Arquitetura AWS - Eficiência de Performance

Este projeto apresenta uma arquitetura AWS de três camadas otimizada para performance, utilizando os princípios do pilar de Eficiência de Performance do AWS Well-Architected Framework. O objetivo é otimizar uma arquitetura existente, adicionando serviços e ajustes para maximizar a performance e a resiliência.

## Grupo 4 – EFICIÊNCIA DE PERFORMANCE

* Michael Jhon Rodrigues Costa
* Gabriele da Conceição Jesus
* José Tadeu Daher
* Guilherme Thomas
* Affonso Souza
* Victor Ramos Andrade Callegari
* Tiago Silva Souza
* Luciano Alves Teles
* Artur de Souza Costa
* Diogo De Assis Luna Da Silva

## Arquitetura Proposta

### Diagrama Estático

![Diagrama Estático](https://github.com/arturcosta86/DESAFIO-DE-ARQUITETURA-PARA-02-11/blob/main/Desafio%20Well-Architected%20Framework%20-%20Resolucao%20Designer%20Efici%C3%AAncia%20de%20Performance%20-%20WAF.png)

### Diagrama Animado (GIF)

![Diagrama Animado](https://github.com/arturcosta86/DESAFIO-DE-ARQUITETURA-PARA-02-11/blob/main/Desafio%20Well-Architected%20Framework%20-%20Resolucao%20Designer%20Efici%C3%AAncia%20de%20Performance%20-%20WAF.gif)

## Descrição da Arquitetura

A arquitetura utiliza os seguintes serviços da AWS, organizados em duas sub-redes (pública e privada) dentro de uma VPC:

**Sub-rede Pública:**

* **CloudFront:** CDN para entrega rápida de conteúdo estático.
* **Application Load Balancer (ALB):** Distribui o tráfego entre os servidores web em múltiplas zonas de disponibilidade (AZs).
* **Auto Scaling (Web Servers):** Escala os servidores web conforme a demanda.
* **Servidores Web:** Recebem as requisições do ALB e as encaminham para a fila SQS.

**Sub-rede Privada:**

* **Servidor de Aplicação (Privado):** Processa a lógica de negócio da aplicação, consome mensagens da fila SQS e acessa os recursos na sub-rede privada.
* **Auto Scaling (Application Servers):** Escala os servidores de aplicação conforme a demanda.
* **ElastiCache (Redis):** Cache em memória para dados frequentemente acessados.
* **RDS (Multi-AZ):** Banco de dados relacional com alta disponibilidade e durabilidade.
* **SQS (Simple Queue Service):** Fila de mensagens que desacopla os servidores web dos servidores de aplicação.

**Serviços Adicionais:**

* **CloudWatch:** Monitora todos os componentes da arquitetura.
* **CloudTrail:**  Registra as chamadas de API feitas à infraestrutura.

## Fluxo de Dados

1. Usuários -> CloudFront -> ALB
2. ALB -> Auto Scaling (Web Servers) -> Servidor Web -> SQS
3. SQS -> Auto Scaling (Application Servers) -> Servidor de Aplicação (Privado) -> ElastiCache
4. ElastiCache -> RDS (Multi-AZ) (somente em caso de *cache miss*)

## Considerações de Performance

* **CloudFront:** Reduz a latência e melhora o tempo de carregamento de conteúdo estático.
* **ALB e Auto Scaling:** Garantem alta disponibilidade e escalabilidade da aplicação.
* **SQS:** Desacopla os servidores web dos servidores de aplicação, permitindo que eles escalem independentemente e aumentando a resiliência da aplicação.
* **ElastiCache:** Melhora a performance da aplicação ao reduzir a carga no banco de dados.
* **RDS Multi-AZ:** Garante alta disponibilidade do banco de dados.
* **Separação de Sub-redes:** Aumenta a segurança e a performance.

## Arquivos do Projeto

* `Desafio Well-Architected Framework - Resolucao Designer Eficiencia de Performance - WAF.drawio`: Arquivo do diagrama no formato diagrams.net (editável).
* `Desafio Well-Architected Framework - Resolucao Designer Eficiência de Performance - WAF.gif`: Arquivo GIF com a animação do fluxo de dados.
* `Desafio Well-Architected Framework - Resolucao Designer Eficiência de Performance - WAF.html`: Arquivo HTML com a animação do fluxo de dados.
* `Desafio Well-Architected Framework - Resolucao Designer Eficiência de Performance - WAF.png`: Arquivo de imagem estática do diagrama.
* `Desafio Well-Architected Framework.drawio`: Arquivo base do diagrama fornecido pelo professor.

## Próximos Passos (Opcional)

* Implementar a arquitetura usando CloudFormation ou Terraform.
* Realizar testes de performance e carga.
* Monitorar e otimizar a arquitetura com base nas métricas do CloudWatch.
