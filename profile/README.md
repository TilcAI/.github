<p align="center">
  <img src="./tilcai-logo.webp" alt="TilcAI logo: an Andean tilcayo (wildcat) head with the TilcAI wordmark" width="320">
</p>

<h3 align="center">Give agents purchasing power. Keep humans in control.</h3>

<p align="center">
  <img alt="Status: early-stage, in development" src="https://img.shields.io/badge/status-early--stage%20%C2%B7%20in%20development-e8ad5c">
  <img alt="Target network: Stellar testnet" src="https://img.shields.io/badge/target-Stellar%20testnet-2cc3cd">
  <img alt="No production funds" src="https://img.shields.io/badge/funds-none%20in%20production-8a9a9c">
</p>

---

**TilcAI** is an SDK and gateway in development for spending policies and trust signals in payments between AI agents — starting on **Stellar**.

The intended flow checks **who** gets paid, **for what** and **within which budget** before an agent can pay. The goal is to allow, block or request human approval under explicit rules, with verifiable decision receipts — not to give an agent unrestricted access to a wallet.

> [!IMPORTANT]
> TilcAI is early-stage. The website and a small policy prototype exist; **there is no deployed contract, published SDK, x402 payment integration or live payment service**. The website demo is a visual simulation and moves no funds. An `ALLOW` result from the prototype does not authorise a transfer.

## What exists today

- **[Landing and architecture](https://tilcai.vercel.app/en)** — public site in [English](https://tilcai.vercel.app/en) and [Spanish](https://tilcai.vercel.app/es).
- **Interactive concept demo** — included in the landing code, with three sample policy scenarios: approved purchase, changed recipient and over-limit amount. Once the latest web build is deployed, it appears at [`/en#demo`](https://tilcai.vercel.app/en#demo). It is a browser-only visual simulation, not a payment or a connection to the core package.
- **`tilcai-core` foundation** — a small, tested TypeScript function that evaluates a normalised payment intent against a policy and returns `ALLOW` or `DENY` with a reason code. It does not authenticate offers, reserve budget atomically, sign receipts or contact Stellar.

## The problem

Agents can already discover and pay for APIs and tools with [x402](https://developers.stellar.org/docs/build/agentic-payments/x402). But a funded wallet is not a mandate:

- **Budgets multiply** — three sub-agents with a 1 USDC limit each can spend 3 USDC, not 1.
- **Limits ignore the purchase** — a per-payment cap does not stop a small payment to a cloned endpoint or a swapped recipient.
- **Instructions can be hijacked** — prompt injection or a retry loop can change the amount, the recipient, or repeat a purchase.
- **No one can explain the payment** — a transaction hash proves a transfer, not who authorised it or why another one was refused.

## How it is meant to work

```mermaid
flowchart LR
    A[Agent] --> I[Payment intent<br/>from the 402 challenge]
    I --> P[Identity + policy]
    P -->|ALLOW| X[x402 + USDC<br/>on Stellar testnet]
    P -->|DENY| R[Decision receipt]
    P -->|REQUIRE_HUMAN| H[Human approval]
    X --> R
```

- **Shared budget tree on Soroban** — a principal funds a root budget and splits it into sub-mandates; a child can never exceed its parent.
- **Deterministic policies** — the model proposes, rules decide. Price and recipient come from the service's 402 challenge, never from model output. Anything unverifiable is denied.
- **Trust signals v0** — a signed provider profile, buyer feedback tied to a paid receipt, and a narrow validation of response format and freshness. Inspired by the concepts of [ERC-8004](https://eips.ethereum.org/EIPS/eip-8004); not an implementation of it.
- **Decision receipts** — signed, with reason codes, for every yes and every no.

## Roadmap

| Stage | Scope | Status |
| --- | --- | --- |
| **Stellar Elite** · mid-October 2026 | Buyer base: budget tree contract (testnet), policy gateway, SDK + MCP tool, reference paid service, receipts, trust signals v0 | In development |
| **HackMeridian** · Oct 25–26, 2026 | Seller extension: signed offers verified against a trusted key and the 402 challenge | Planned · subject to event acceptance |
| **Vision** | Agent-to-business commerce: bookings, cancellations and refunds between agents of people and small businesses | Not scheduled |

A signature alone never authorises a payment: the buyer also checks that the seller's key was already trusted, that the terms match the 402 challenge, and that the spending policy still allows it.

## Planned stack

`Stellar Testnet` · `Soroban` · `USDC (SEP-41)` · `x402 exact` · `TypeScript` · `MCP` · `Next.js`

Listing a technology does not imply partnership, sponsorship or a finished integration. No support for other networks is claimed.

## Repositories

| Repository | Current scope | Access |
| --- | --- | --- |
| [`tilcai-web`](https://github.com/TilcAI/tilcai-web) | Public landing, proposed architecture and visual policy demo | Public |
| `tilcai-core` | Minimal policy-evaluation prototype with tests; no payment integration | Private to the team for now |

The website demo and core prototype are separate today. Connecting them is future implementation work, not a current feature.

---

<details>
<summary><b>Español</b></summary>

**TilcAI** es un SDK y gateway en desarrollo para políticas de gasto y señales de confianza en pagos entre agentes de IA, empezando por **Stellar**. El flujo propuesto comprobará a quién se paga, por qué y con qué presupuesto antes de permitir, bloquear o pedir aprobación humana. Los recibos firmados aún no están implementados.

Ya existen la [web pública](https://tilcai.vercel.app/es), una demo conceptual incluida en su código (visible en [`/es#demo`](https://tilcai.vercel.app/es#demo) cuando se despliegue la última versión) y una pequeña base de código de políticas con pruebas (`tilcai-core`, privado por ahora). La demo no mueve fondos ni usa todavía ese núcleo. No hay contrato desplegado, SDK publicado, integración de pagos x402 ni servicio en producción. La base compradora se construye durante **Stellar Elite** (mediados de octubre de 2026) y la ampliación con ofertas firmadas de negocios está prevista para **HackMeridian** (25–26 de octubre de 2026), sujeta a la aceptación en el evento.

</details>
