# EtaFome — Arquitetura

Plataforma de pedidos para restaurantes/lanchonetes (modelo marketplace), com dois apps
de cliente final e um backend compartilhado.

## Visão geral

```
                        ┌────────────────────────┐
                        │          API           │  Node.js + Express + TypeScript
                        │  REST + WebSocket      │  PostgreSQL
                        │                        │  Socket.io (tempo real)
                        └───────────┬────────────┘
                     ┌──────────────┴──────────────┐
                     │                             │
         ┌───────────▼──────────────┐  ┌───────────▼──────────────┐
         │ App do cliente           │  │ Painel do estabelecimento│
         │ React Native (Expo)      │  │ Next.js (web)            │
         │ Cliente final pede       │  │ Loja gerencia cardápio,  │
         │ comida                   │  │ pedidos e fila           │
         └──────────────────────────┘  └──────────────────────────┘
```

**Decisão:** o lado do estabelecimento nasce como painel **web**, não app mobile.
Quem opera o balcão/cozinha usa navegador em tablet/PC — é mais prático de manter
(uma base de código, sem publicação em loja de apps) e cobre bem o caso de uso de
"gerenciar cardápio, pedidos e fila". Um app mobile para o dono pode vir no futuro,
reaproveitando a mesma API.

## Stack

| Camada | Tecnologia | Motivo |
|---|---|---|
| App cliente | React Native + Expo + TypeScript | iOS/Android com uma base de código |
| Painel loja | Next.js + TypeScript | Dashboard web responsivo |
| Backend | Node.js + Express + TypeScript | Simples, produtivo, mesma linguagem do front |
| Banco de dados | PostgreSQL + Prisma | Migrations tipadas, bom encaixe com TS |
| Tempo real | Socket.io | Status de pedido e fila ao vivo, para cliente e loja |

## Domínio (conceitos principais)

- **Usuário** — cliente ou dono/funcionário de loja
- **Estabelecimento** — o restaurante/lanchonete
- **Assinatura** — plano mensal do estabelecimento; controla se ele pode operar
- **Cardápio** — categorias e itens, com preço e disponibilidade
- **Pedido** — feito pelo cliente, passa por recebido → em preparo → saiu para entrega → concluído (ou cancelado)
- **Fila** — os pedidos ativos do estabelecimento, em ordem de chegada

## Fluxo do pedido (tempo real)

1. Cliente monta o carrinho no app e finaliza o pedido.
2. Após a confirmação do pagamento, o pedido entra na fila da loja em tempo real.
3. A loja aceita, recusa ou avança o status do pedido.
4. Cada mudança é enviada na hora para o cliente e para o painel, sem recarregar.

## Cadastro automático de estabelecimento

- O dono se cadastra no painel web e preenche os dados da loja.
- Ao confirmar a assinatura, a loja é ativada automaticamente e passa a aparecer no app.
- Se a assinatura fica inadimplente ou é cancelada, a loja sai da listagem automaticamente.
