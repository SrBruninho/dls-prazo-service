# Distributed Logistics System - Prazo Service

Objetivo

Simular um sistema distribuído de logística com microservices, filas e streaming de eventos, focando em resiliência e integração de bancos diferentes.

Tecnologias

Java 21+, Spring Boot 3+

Kafka ou RabbitMQ

MongoDB + PostgreSQL

Docker Compose

Testcontainers para testes automatizados

Arquitetura e Microservices

2️⃣ Cálculo de Prazo Service

Função: Recebe eventos de novos pedidos e calcula previsão de entrega.
Processamento:

Consumidor Kafka/RabbitMQ escuta eventos do Pedidos Service

Calcula tempo estimado baseado em origem/destino e itens

Publica evento de “prazo calculado” para Notificações Service

Banco: a definir (para armazenamento temporário de cálculos ou logs)

```mermaid
flowchart LR

    %% Serviços
    subgraph PedidoService["pedido-service"]
        PS[pedido-service]
    end

    subgraph PrazoService["prazo-service"]
        PR[prazo-service]
    end

    subgraph NotificacaoService["notificacao-service"]
        NS[notificacao-service]
    end

    %% Cliente e provedor de email
    Cliente((Cliente))
    EmailProvider["Email Provider<br/>(SendGrid / SES / SMTP)"]

    %% Bancos
    PSDB[(pedido-db<br/>MongoDB)]
    PRDB[(prazo-db<br/>Postgres)]
    NSDB[(notificacao-db<br/>Postgres)]

    %% Fluxo dos serviços
    PS -->|novo pedido| PR
    PR -->|prazo calculado| NS

    %% Ligações com bancos
    PS --> PSDB
    PR --> PRDB
    NS --> NSDB

    %% Envio de email
    NS -->|enviar e-mail| EmailProvider --> Cliente

```
