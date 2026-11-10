# Historias de usuario

**Actor principal:** Cliente del banco que usa el cajero automático.

## HU-01: Autenticarme con mi PIN

**Como** cliente, **quiero** insertar mi tarjeta e ingresar mi PIN, **para** acceder de forma segura a mis cuentas.

**Criterios de aceptación**
- Con el PIN correcto se muestra el menú principal.
- Con un PIN incorrecto se muestra un mensaje de error y se registra el intento fallido.

**Prioridad:** Alta

## HU-02: Bloqueo por intentos fallidos

**Como** cliente, **quiero** que mi tarjeta se bloquee tras 3 PIN incorrectos, **para** evitar accesos no autorizados.

**Criterios de aceptación**
- Al tercer intento fallido consecutivo la tarjeta cambia a estado BLOQUEADA.
- Una tarjeta bloqueada no permite ninguna operación y se informa al cliente.

**Prioridad:** Alta

## HU-03: Consultar saldo

**Como** cliente, **quiero** ver el saldo de mi cuenta, **para** saber cuánto dinero tengo disponible.

**Criterios de aceptación**
- El saldo se muestra en pantalla después de autenticarme.
- Consultar el saldo no modifica el valor de la cuenta.

**Prioridad:** Alta

## HU-04: Retirar efectivo

**Como** cliente, **quiero** retirar dinero en efectivo, **para** disponer de él sin ir a una sucursal.

**Criterios de aceptación**
- No se permite retirar un monto mayor al saldo de la cuenta.
- No se permite retirar un monto mayor al efectivo disponible en el cajero.
- Tras un retiro exitoso el saldo se descuenta y se entrega el efectivo.

**Prioridad:** Alta

## HU-05: Depositar dinero

**Como** cliente, **quiero** depositar dinero en mi cuenta, **para** aumentar mi saldo desde el cajero.

**Criterios de aceptación**
- El monto depositado debe ser mayor que cero.
- Tras el depósito el saldo se incrementa en el monto ingresado.

**Prioridad:** Media

## HU-06: Transferir a otra cuenta

**Como** cliente, **quiero** transferir dinero a otra cuenta, **para** pagar o enviar dinero a otra persona.

**Criterios de aceptación**
- La cuenta destino debe existir; si no, se muestra un error.
- El monto no puede superar el saldo disponible.
- Se descuenta de la cuenta origen y se acredita en la cuenta destino.

**Prioridad:** Media

## HU-07: Obtener comprobante

**Como** cliente, **quiero** recibir un comprobante de mi operación, **para** tener constancia de ella.

**Criterios de aceptación**
- Al terminar una transacción se ofrece imprimir el comprobante.
- El comprobante muestra tipo de operación, monto, fecha y saldo resultante.

**Prioridad:** Media

## HU-08: Finalizar la sesión

**Como** cliente, **quiero** cerrar mi sesión y recuperar mi tarjeta, **para** que nadie más use mi cuenta.

**Criterios de aceptación**
- Al elegir Salir se devuelve la tarjeta y se vuelve a la pantalla inicial.
- Si no hay actividad durante 60 segundos la sesión se cierra automáticamente.

**Prioridad:** Media
