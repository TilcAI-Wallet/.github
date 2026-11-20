# TilcAI Wallet

**Compras verificables para agentes de IA en Stellar.**

TilcAI Wallet permite conectar un agente, fondear una tarjeta para su sesión y controlar sus compras desde el celular. Cada recibo vincula el permiso del dueño, la orden, el pago confirmado y la evidencia de lo que entregó el proveedor.

*Tu wallet. Tus agentes. Tú pones los límites.*

## El problema

Cuando un agente compra un informe o consume una API de pago, el negocio necesita reconstruir quién autorizó el gasto, qué se pidió, a quién se pagó y qué se recibió. A medida que crecen las sesiones y los proveedores, esa información queda repartida entre clientes, transacciones y sistemas de entrega.

## Cómo funciona

1. **Conecta** una instalación de un agente y aprueba su sesión desde la wallet.
2. **Fondea** su tarjeta desde la cuenta principal. Fija el máximo por compra, el tope acumulado, los destinos admitidos y el vencimiento.
3. **Revisa** la orden, el pago en Stellar y la evidencia de entrega desde la actividad de la tarjeta.
4. **Controla** el acceso desde el móvil. Puedes cerrar una sesión, congelar nuevas compras o revocar el permiso y recuperar el remanente.

### Centro de sesiones

La vista de **Sesiones** reúne accesos de web y móvil, vínculos de canales y clientes de agentes. Cada fila muestra cliente, instalación, estado y última actividad. La primera integración combina web móvil y MCP. La visión del producto incluye Codex, Claude, OpenCode, Telegram, WhatsApp según el flujo permitido, extensión de navegador, CLI y TUI.

### Una tarjeta por sesión de agente

Cada sesión habilitada para gastar tiene una subcuenta Stellar propia. La vista principal consolida los saldos y conserva el disponible de la cuenta principal. La tarjeta muestra saldo, gasto, compras en curso, límites y extracto. Dos instalaciones del mismo agente pueden operar con presupuestos independientes.

### Control de acceso y gasto

El diseño conserva la autoridad del dueño mediante una passkey. El firmante delegado de la sesión opera dentro de las reglas de su subcuenta. Cerrar el acceso invalida la credencial del cliente. Congelar bloquea nuevas solicitudes y el panel indica cuándo se confirma el cambio en Stellar. La revocación retira el permiso de gasto y permite recuperar el saldo una vez conciliados los pagos pendientes.
