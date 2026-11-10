# Diagramas UML del Cajero Automático

Los diagramas están escritos en [Mermaid](https://mermaid.js.org/), por lo que GitHub los muestra como imagen directamente en esta página.

## 1. Diagrama de casos de uso

```mermaid
flowchart LR
    cliente(("Cliente"))

    subgraph sistema["Sistema Cajero"]
        CU1([Autenticarse con PIN])
        CU2([Consultar saldo])
        CU3([Retirar efectivo])
        CU4([Depositar])
        CU5([Transferir])
        CU6([Imprimir comprobante])
        CU7([Cambiar PIN])
    end

    cliente --- CU2
    cliente --- CU3
    cliente --- CU4
    cliente --- CU5
    cliente --- CU7

    CU2 -.->|include| CU1
    CU3 -.->|include| CU1
    CU4 -.->|include| CU1
    CU5 -.->|include| CU1
    CU7 -.->|include| CU1
    CU3 -.->|extend| CU6
    CU4 -.->|extend| CU6
    CU5 -.->|extend| CU6
```

## 2. Diagrama de clases

```mermaid
classDiagram
    direction LR

    class Cliente {
        -int id
        -String nombre
        -String documento
    }

    class Tarjeta {
        -String numero
        -String pinHash
        -Date vencimiento
        -int intentosFallidos
        -EstadoTarjeta estado
        +validarPin(String pin) boolean
        +registrarIntentoFallido() void
        +bloquear() void
    }

    class EstadoTarjeta {
        <<enumeration>>
        ACTIVA
        BLOQUEADA
        VENCIDA
    }

    class Cuenta {
        -String numeroCuenta
        -double saldo
        -String tipo
        +consultarSaldo() double
        +depositar(double monto) void
        +retirar(double monto) boolean
    }

    class Cajero {
        -String id
        -String ubicacion
        -double efectivoDisponible
        +autenticar(Tarjeta tarjeta, String pin) boolean
        +mostrarMenu() void
        +dispensarEfectivo(double monto) boolean
        +cerrarSesion() void
    }

    class Transaccion {
        <<abstract>>
        #int id
        #Date fecha
        #double monto
        +ejecutar() boolean*
    }

    class Retiro
    class Deposito
    class Transferencia {
        -String cuentaDestino
    }
    class ConsultaSaldo

    class Comprobante {
        -int numero
        -Date fecha
        -String detalle
        +imprimir() void
    }

    Cliente "1" --> "1..*" Tarjeta : posee
    Cliente "1" --> "1..*" Cuenta : es titular de
    Tarjeta "1" --> "1..*" Cuenta : da acceso a
    Tarjeta --> EstadoTarjeta
    Cajero "1" --> "*" Transaccion : procesa
    Transaccion "*" --> "1" Cuenta : afecta
    Transaccion "1" --> "0..1" Comprobante : genera
    Transaccion <|-- Retiro
    Transaccion <|-- Deposito
    Transaccion <|-- Transferencia
    Transaccion <|-- ConsultaSaldo
```

### Decisiones de diseño

- **Herencia y polimorfismo:** `Retiro`, `Deposito`, `Transferencia` y `ConsultaSaldo` heredan de `Transaccion` y cada una implementa su propio `ejecutar()`.
- **Seguridad:** el PIN no se guarda en texto plano (`pinHash`) y la tarjeta se bloquea tras 3 intentos fallidos.
- **Responsabilidades separadas:** `Cuenta` controla el saldo, `Cajero` controla el efectivo físico y la interacción con el cliente.

## 3. Diagrama de secuencia: retirar efectivo

```mermaid
sequenceDiagram
    actor U as Cliente
    participant C as Cajero
    participant T as Tarjeta
    participant A as Cuenta

    U->>C: Inserta tarjeta
    C-->>U: Solicita PIN
    U->>C: Ingresa PIN
    C->>T: validarPin(pin)

    alt PIN incorrecto
        T-->>C: false
        C->>T: registrarIntentoFallido()
        C-->>U: Muestra error (bloqueo al tercer intento)
    else PIN correcto
        T-->>C: true
        U->>C: Selecciona Retirar e indica el monto
        C->>A: retirar(monto)

        alt Saldo insuficiente
            A-->>C: false
            C-->>U: Fondos insuficientes
        else Saldo suficiente
            A-->>C: true
            C->>C: dispensarEfectivo(monto)
            C-->>U: Entrega efectivo y ofrece comprobante
        end
    end
```
