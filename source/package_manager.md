
# Módulo `package_manager`

## Resumen

El módulo `spike_staking::package_manager` es un contrato de utilidad interno diseñado para gestionar permisos y direcciones de sistema dentro del paquete de staking.

Su función principal es crear y mantener una cuenta de recursos central para el paquete y proporcionar acceso controlado a su `signer` a otros módulos autorizados (declarados como `friend`). También actúa como un registro para almacenar y recuperar direcciones de sistema importantes.

## Funcionalidades Clave

-   **Gestión de Cuenta de Recursos**: En su inicialización, crea una cuenta de recursos para el paquete de staking. Esta cuenta puede poseer activos u otros recursos en nombre del protocolo.
-   **Acceso a Firmante (Signer)**: Proporciona una función `get_signer()` que permite a los módulos `friend` (como `coin_wrapper` y `router_stake`) obtener el `signer` de la cuenta de recursos. Esto es crucial para que esos módulos puedan realizar acciones privilegiadas, como mover fondos que pertenecen al protocolo.
-   **Registro de Direcciones**: Ofrece una manera de almacenar y consultar direcciones de sistema por nombre (un `String`). Esto evita tener que codificar direcciones directamente en los contratos y permite una mayor flexibilidad.

## Estructuras (`structs`)

### `PermissionConfig`

Un recurso que se almacena en la cuenta principal del paquete (`@spike_staking`) y contiene:

-   `signer_cap: SignerCapability`: La capacidad que permite generar el `signer` de la cuenta de recursos.
-   `addresses: SmartTable<String, address>`: Una tabla inteligente que mapea nombres (`String`) a direcciones (`address`).

## Funciones

### Inicialización

-   `init_module(swap_signer: &signer)`: Se llama una sola vez durante el despliegue del paquete. Crea la cuenta de recursos y el recurso `PermissionConfig`.

### Funciones de Amigo (`public(friend) fun`)

-   `get_signer(): signer`: Devuelve el `signer` de la cuenta de recursos. Solo puede ser llamada por módulos `friend`.
-   `add_address(name: String, object: address)`: Almacena una nueva dirección en el registro. Solo puede ser llamada por módulos `friend`.

### Funciones Públicas (`public fun`)

-   `address_exists(name: String): bool`: Verifica si una dirección con un nombre específico ya está registrada.
-   `get_address(name: String): address`: Recupera una dirección del registro a partir de su nombre.

