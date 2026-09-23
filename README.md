<div align="center">

<img src="assets/logo.png" alt="Muita Fome" width="120" />

# Muita Fome

**Peça comida de restaurantes e lanchonetes perto de você — e gerencie seu estabelecimento em tempo real.**

[![Node.js](https://img.shields.io/badge/Node.js-20+-339933?logo=node.js&logoColor=white)](https://nodejs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![React Native](https://img.shields.io/badge/React%20Native-Expo-000020?logo=expo&logoColor=white)](https://expo.dev)
[![Next.js](https://img.shields.io/badge/Next.js-14-000000?logo=next.js&logoColor=white)](https://nextjs.org)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?logo=postgresql&logoColor=white)](https://www.postgresql.org)
[![Socket.io](https://img.shields.io/badge/Socket.io-realtime-010101?logo=socket.io&logoColor=white)](https://socket.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

</div>

---

## O que é o Muita Fome

Muita Fome é uma plataforma de pedidos de comida no modelo **marketplace** — como o iFood —
conectando **clientes** que querem pedir comida a **restaurantes, lanchonetes e outros
estabelecimentos** que gerenciam seu cardápio, pedidos e fila de preparo em tempo real.

A plataforma tem duas frentes:

- 📱 **App do cliente** (mobile) — busca estabelecimentos, monta o pedido, acompanha o
  status ao vivo, avalia depois de receber.
- 💻 **Painel do estabelecimento** (web) — cardápio, fila de pedidos, aceitar/recusar,
  cupons, configurações da loja — tudo em tempo real, sem precisar dar F5.

Novos estabelecimentos se cadastram sozinhos e entram no ar automaticamente assim que a
assinatura mensal é confirmada — sem depender de um time comercial para aprovar cadastro.

## Funcionalidades

**Cliente (app mobile)**
- Busca e filtro de estabelecimentos por nome/categoria, com indicação de loja aberta/fechada
- Cardápio com categorias, itens, opções/variações (ex: tamanho, adicionais) e preço por escolha
- Carrinho, endereço de entrega e cupom de desconto (percentual ou valor fixo)
- Acompanhamento do pedido em tempo real (recebido → em preparo → saiu para entrega → concluído)
- Histórico de pedidos com "pedir de novo"
- Avaliação (nota + comentário) depois do pedido concluído
- Notificações push nas mudanças de status

**Estabelecimento (painel web)**
- Cadastro e assinatura mensal (ativação automática da loja ao confirmar pagamento)
- Fila de pedidos em tempo real, separada por categoria (Recebido, Em preparo, Saiu para
  entrega, Concluído)
- Aceitar ou recusar pedido com motivo; cancelamento com motivo dos dois lados
- Pedido só chega na fila da loja depois que o pagamento é confirmado
- Gestão de cardápio (categorias, itens, disponibilidade, opções/variações)
- Cupons de desconto e taxa de entrega configurável
- Abrir/fechar a loja manualmente (cliente não consegue pedir de loja fechada)

## Como funciona (tempo real)

```
Cliente monta o pedido  ──▶  Pagamento confirmado (webhook)  ──▶  Pedido entra na fila da loja
                                                                          │
                                                                    Socket.io
                                                                          │
        Cliente acompanha status ◀── loja aceita/recusa/avança status ◀──┘
```

O backend expõe REST + WebSocket (Socket.io): assim que a loja aceita, recusa ou avança
um pedido, tanto o cliente quanto o painel da loja recebem a atualização instantaneamente,
sem precisar recarregar a página ou o app.

## Stack técnica

| Camada | Tecnologia |
|---|---|
| App do cliente | React Native + Expo + TypeScript |
| Painel do estabelecimento | Next.js + TypeScript |
| API | Node.js + Express + TypeScript |
| Banco de dados | PostgreSQL + Prisma ORM |
| Tempo real | Socket.io |
| Autenticação | JWT |
| Notificações | Expo Push Notifications |

## Documentação

Veja [`ARCHITECTURE.md`](ARCHITECTURE.md) para uma visão geral da arquitetura.

## Roadmap

Próximos passos planejados:

- Geolocalização (busca de estabelecimentos por proximidade)
- Upload de imagens do cardápio
- Integração com gateway de pagamento

## Licença

Distribuído sob a licença MIT. Veja em [`LICENSE`](LICENSE) para mais detalhes.
