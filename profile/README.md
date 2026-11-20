<p align="center">
  <img src="./assets/banner.svg" alt="TilcAI Wallet: compras verificables para agentes de IA en Stellar" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Stellar-testnet-51C5FB?style=for-the-badge&logo=stellar&logoColor=white" alt="Stellar testnet">
  <img src="https://img.shields.io/badge/Soroban-smart%20accounts-6024E8?style=for-the-badge" alt="Soroban smart accounts">
  <img src="https://img.shields.io/badge/x402-pagos%20ag%C3%A9nticos-925FF6?style=for-the-badge" alt="x402">
  <img src="https://img.shields.io/badge/MCP-agentes-416CD5?style=for-the-badge" alt="MCP">
  <img src="https://img.shields.io/badge/Stellar%20Bolivia-2026-1F1A57?style=for-the-badge" alt="Stellar Bolivia 2026">
</p>

<h3 align="center">Tus agentes compran. Tú sabes qué autorizaste, cuánto pagaron y qué recibiste.</h3>

TilcAI Wallet permite conectar un agente, fondear una **tarjeta** para su sesión y controlar sus compras desde el celular. Cada recibo vincula el permiso del dueño, la orden, el pago confirmado en Stellar y la evidencia de lo que entregó el proveedor.

> [!NOTE]
> **Proyecto en diseño e integración.** Las funciones descritas aquí son el objetivo del producto. La sección [Estado y validación](#-estado-y-validación) indica qué está comprobado en testnet.

---

## 🤔 El problema

Un agente de IA ya puede comprar un informe o consumir una API de pago en segundos. Lo difícil viene después, cuando el negocio tiene que responder:

| ❓ | La pregunta | Dónde suele quedar la respuesta |
| :---: | --- | --- |
| 🔐 | ¿Quién autorizó este gasto? | En la configuración de algún cliente |
| 📝 | ¿Qué pidió el agente? | En el historial de una conversación |
| 💸 | ¿A quién se pagó y cuánto? | En una transacción en cadena |
| 📦 | ¿Qué se recibió a cambio? | En el sistema del proveedor |

Con varias sesiones y varios proveedores, esa información queda repartida. **TilcAI Wallet la reúne en un solo recibo.**

## ⚡ Cómo funciona

| Paso | | Qué haces |
| :---: | :---: | --- |
| **1** | 🔌 **Conecta** | Vincula una instalación de un agente y aprueba su sesión desde la wallet. |
| **2** | 💳 **Fondea** | Pasa saldo de tu cuenta principal a la tarjeta del agente. Fija el máximo por compra, el tope acumulado, los destinos admitidos y el vencimiento. |
| **3** | 🧾 **Revisa** | Sigue la orden, el pago en Stellar y la evidencia de entrega desde la actividad de la tarjeta. |
| **4** | 🛑 **Controla** | Desde el móvil, cierra una sesión, congela nuevas compras o revoca el permiso y recupera el remanente. |

### 🪪 Una tarjeta por sesión de agente

Cada sesión habilitada para gastar tiene **su propia subcuenta Stellar**. El agente solo puede gastar lo que hay en su tarjeta, y dos instalaciones del mismo agente pueden operar con presupuestos independientes. La vista principal consolida los saldos y conserva el disponible de la cuenta principal.

```text
┌──────────────────────────────────────┐
│ TilcAI                       AG-7K2Q │
│ Codex · Portátil        ● Activa     │
│                                      │
│ 24,50 USDC                 de 25,00  │
│ ███████████████████████████████████░ │
│ Máx. 0,50 por compra   1 proveedor   │
└──────────────────────────────────────┘
```

### 🧭 Centro de sesiones

La vista **Sesiones** reúne en un solo lugar los accesos de web y móvil, los vínculos de canales y los clientes de agentes. Cada fila muestra cliente, instalación, estado y última actividad, y se puede cerrar por separado.

La primera integración combina **web móvil + MCP**. La visión incluye Codex, Claude, OpenCode, Telegram, WhatsApp (según el flujo permitido), extensión de navegador, CLI y TUI.

### 🔑 Control de acceso y gasto

- **La autoridad es tuya.** El dueño controla la cuenta con una passkey. El agente nunca la ve.
- **El agente firma dentro de un marco.** El firmante delegado de cada sesión solo opera dentro de las reglas de su subcuenta.
- **Tres niveles para detenerlo:**
  - **Cerrar el acceso** invalida la credencial del cliente.
  - **Congelar** bloquea nuevas solicitudes; el panel indica cuándo se confirma el cambio en Stellar.
  - **Revocar** retira el permiso de gasto y devuelve el saldo una vez conciliados los pagos pendientes.

## 🧾 Qué hace verificable una compra

```mermaid
flowchart LR
    A["🔐 Permiso<br/>sesión, topes, vigencia"] --> B["📝 Orden<br/>servicio, importe, destino"]
    B --> C["💸 Pago<br/>hash confirmado en Stellar"]
    C --> D["📦 Entrega<br/>evidencia del proveedor"]
```

| Evidencia | Qué permite revisar |
| --- | --- |
| **Permiso** | Sesión, tarjeta, proveedor admitido, topes y vigencia aprobados. |
| **Orden** | Servicio solicitado, importe y destinatario. |
| **Pago** | Activo, importe, red, hash y confirmación en Stellar. |
| **Entrega** | Referencia y estado del servicio reportado por el proveedor. |

Un pago confirmado no significa que el servicio llegó. Por eso la wallet distingue los estados:

**Pago pendiente** → **Pagado, pendiente de entrega** → **Entregado**

El recibo conserva el mismo identificador al pasar de MCP al panel móvil.

## 🎬 Un ejemplo en testnet

Una agencia fondea **25 USDC de prueba** para la sesión **Codex · Portátil**, con un máximo de **0,50 USDC por informe** y un solo proveedor autorizado.

```mermaid
sequenceDiagram
    actor D as 📱 Dueño
    participant A as 🤖 Codex por MCP
    participant W as TilcAI Wallet
    participant S as ⭐ Stellar
    participant P as 🏢 Proveedor
    D->>W: Fondea la tarjeta con 25 USDC y fija los límites
    A->>W: Pide un informe de 0,50 USDC
    W->>W: Verifica sesión, tope y destino
    W->>S: Paga desde la subcuenta de la tarjeta
    S-->>W: Pago confirmado
    W->>P: Solicita la entrega
    P-->>W: Evidencia del informe
    W-->>D: Recibo completo · saldo 24,50 USDC
    D->>W: Congela la tarjeta
    A->>W: Nuevo intento de compra
    W-->>A: ❌ Rechazado
```

Las comisiones se suponen patrocinadas fuera del saldo de la tarjeta.

## 🎯 Para quién

Para **agencias de software, fintechs y equipos** que delegan compras recurrentes de servicios digitales en agentes:

- 🔎 Informes de debida diligencia
- 🔌 Consultas a APIs de pago
- 🛠️ Herramientas por consumo

El valor se mide con **compras conciliadas**, **evidencia de entrega** y **el tiempo que toma revisar cada gasto**.

## 🏗️ Arquitectura

```mermaid
flowchart LR
    M["📱 Panel web móvil"] --> G
    C["🤖 Agentes por MCP"] --> G
    G["TilcAI Wallet<br/>gateway"] --> SA["⭐ Subcuentas y reglas<br/>en Soroban"]
    G --> P["🏢 Proveedor"]
    SA --> R["Conciliación<br/>con el ledger"]
    P --> R
    R --> M
```

- **Stellar** aporta cuentas de contrato, liquidación y políticas de autorización.
- **El gateway** valida la sesión, coordina la orden y concilia la transacción con el proveedor.
- **La interfaz** recibe eventos de actividad y muestra el estado a partir de confirmaciones del ledger.
- **Pagos:** se evalúa la compatibilidad de x402 con la subcuenta, con liquidación directa en testnet durante la validación.

📚 [Cuentas de contrato en Stellar](https://developers.stellar.org/docs/build/guides/contract-accounts) · [x402 en Stellar](https://developers.stellar.org/docs/build/agentic-payments/x402/quickstart-guide)

### 📦 Monorepo de producto

| Área | Responsabilidad |
| --- | --- |
| **Apps** | Panel web móvil, cliente MCP y adaptadores de canal. |
| **Servicios** | API, vinculación, órdenes y conciliación. |
| **Paquetes** | Dominio wallet, contratos de API, SDK y adaptador a TilcAI. |
| **Contratos** | Integración de smart accounts y reglas de gasto en Soroban. |
| **Pruebas** | Aislamiento entre sesiones, liquidación, duplicados, congelación y recuperación. |

### 🧱 Construido sobre TilcAI

No partimos de cero. El producto reutiliza la base de **[TilcAI](https://github.com/TilcAI/tilcai-infrastructure)** para cuentas, WebAuthn, pagos x402, relayer y monitorización. El monorepo añade la experiencia de wallet, el ciclo de vida de sesiones y tarjetas, y la evidencia de compra. Las rutas de fondeo adicionales se habilitan después de verificar la red, el activo y la conciliación.

## 🚦 Estado y validación

**Primera validación:** reproducir un ciclo completo en Stellar testnet.

- [ ] Desplegar y fondear una subcuenta
- [ ] Pagar desde MCP
- [ ] Consultar el recibo
- [ ] Congelar la tarjeta y comprobar que se rechaza un nuevo intento
- [ ] Revocar el permiso y recuperar el remanente

**Siguiente etapa:** dos tarjetas independientes, reconexión de sesiones y continuidad con más canales. Cada integración se publica como disponible al completar sus pruebas.

## 🇧🇴 Stellar Bolivia

TilcAI Wallet participa en la **[National Startups Hackathon 2026](https://www.stellar-bolivia.xyz/)** de Stellar Bolivia como una herramienta de control y conciliación para equipos que compran servicios mediante agentes.

---

## 🌎 English summary

**TilcAI Wallet** is a Stellar wallet for accountable AI agent purchases. Each spending-enabled agent session gets its own funded smart-account card with its own limits. Owners manage sessions and spending from mobile, while every purchase receipt links **permission → order → settlement → delivery**. The product is designed as a monorepo built on the existing TilcAI infrastructure.

<p align="center">
  <sub><i>Tu wallet. Tus agentes. Tú pones los límites.</i></sub>
</p>
