# Llamadas Privadas

## Información de la cuenta

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.account_info()
```

> The above command returns JSON structured like this:

```json
{
  "id": 548,
  "level_id": 3
  "volume_30d": 22584.25
}
```

Información de la cuenta

### HTTP Request

`GET http://example.com/api/kittens`

### Path Parameters

Parameter | Description
--------- | -----------
user_id | La ID del usuario

### Response Details

Key | Type | Description
--------- | --------- | ---------
id | [int] | id de la cuenta
level_id | [string] | Nivel de verificación de la cuenta
volume_30d | [amount] | Volumen transado en los ultimos 30 días


<aside class="success">
Idea de endpoint
</aside>


## Balances

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.balance()
```

> The above command returns JSON structured like this:

```json
{
  "balances": [
    {
      "account_id": 358,
      "amount": ["25.77442722", "BTC"],
      "available_amount": ["11.52442722", "BTC"],
      "frozen_amount": ["10.0", "BTC"],
      "id": "BTC",
      "pending_withdraw_amount": ["4.25", "BTC"]
    },
    {
      "account_id": 358,
      "amount": ["33088710.56", "CLP"],
      "available_amount": ["24197707.52", "CLP"],
      "frozen_amount": ["7000000.04", "CLP"],
      "id": "CLP",
      "pending_withdraw_amount": ["1891003.0", "CLP"]
    },
    {
      "account_id": 358,
      "amount": ["11354811.23", "COP"],
      "available_amount": ["11354811.23", "COP"],
      "frozen_amount": ["0.0", "COP"],
      "id": "COP",
      "pending_withdraw_amount": ["0.0", "COP"]
    }
  ]
}
```

Muestra los balances de tu cuenta

### HTTP Request

`GET /balances`

`GET /balances/<currency>`

### Path Parameters

Parameter | Description
--------- | -----------
currency | *Opcional - El acrónimo de la mondea (Ej: "btc", "clp", "cop")

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted

### Response Details (JSON)

Key | Type | Description
--------- | --------- | ---------
balance | [array] | Arreglo de balances por moneda
id | [currency] | Moneda del arreglo de balances
amount | [amount_string, currency] | Cantidad total asociado a la cuenta
available_amount | [amount_string, currency] | Cantidad disponible
frozen_amount | [amount_string, currency] | Cantidad retenida en órdenes pendientes
pending_withdrawal_amount | [amount_string, currency] | Cantidad retenida en proceso de retiro
created_at | [time] | Fecha de creación
updated_at | [time] | Fecha de la última actualización

<aside class="success">
Recuerda — Sólo el balance disponible sirve para crear nuevas órdenes
</aside>

## Mis Órdenes

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.orders("btc-clp")
```

> The above command returns JSON structured like this:

```json
{
  "meta": {
    "current_page": 1,
    "total_count": 42899,
    "total_pages": 14300
  },
    "orders": [
    {
      "account_id": 358,
      "amount": ["0.8317892", "BTC"],
      "created_at": "2017-04-18T16:09:38.089Z",
      "fee_currency": "CLP",
      "id": 620195,
      "limit": ["838305.78", "CLP"],
      "market_id": 1,
      "original_amount": ["0.8317892", "BTC"],
      "paid_fee": ["0.0", "CLP"],
      "price_type": "limit",
      "state": "pending",
      "total_exchanged": ["0.0", "CLP"],
      "traded_amount": ["0.0", "BTC"],
      "type": "Ask",
      "weighted_quotation": None
    },
    {
      "account_id": 358,
      "amount": ["1.4527277", "BTC"],
      "created_at": "2017-04-18T16:09:36.766Z",
      "fee_currency": "CLP",
      "id": 620194,
      "limit": ["838000.57", "CLP"],
      "market_id": 1,
      "original_amount": ["1.4527277", "BTC"],
      "paid_fee": ["0.0", "CLP"],
      "price_type": "limit",
      "state": "pending",
      "total_exchanged": ["0.0", "CLP"],
      "traded_amount": ["0.0", "BTC"],
      "type": "Ask",
      "weighted_quotation": None
    },
    {
      "account_id": 358,
      "amount": ["1.40988433", "BTC"],
      "created_at": "2017-04-18T16:09:35.498Z",
      "fee_currency": "CLP",
      "id": 620193,
      "limit": ["837858.51", "CLP"],
      "market_id": 1,
      "original_amount": ["1.40988433", "BTC"],
      "paid_fee": ["0.0", "CLP"],
      "price_type": "limit",
      "state": "pending",
      "total_exchanged": ["0.0", "CLP"],
      "traded_amount": ["0.0", "BTC"],
      "type": "Ask",
      "weighted_quotation": None
    }
  ]
}
```

La orden es el núcleo del Exchange. Una orden es una oferta de compra o de venta (dependiendo del type) de un cierto monto (amount) de la moneda base del mercado (posiblemente BTC). Si la orden es una orden de mercado (usando el atributo price_type en market), intentará comprar o vender al mejor precio disponible en el mercado. En caso contrario, deberá enviarse el precio al que se colocará la orden (usando limit), que será el mejor límite al que se desee comprar o vender.

La ejecución de la orden es programada para ser ejecutada inmediatamente luego de su creación, aunque el proceso ocurre asíncronamente y la orden podría quedar parcialmente comprada/vendida durante una cierta cantidad de tiempo. Durante el ciclo de vida de una orden, ésta puede pasar por varios estados:

State | Description
--------- | -----------
received | La orden ha sido recibida pero no ha ocurrido ningún procesamiento aún
pending | El monto máximo que la orden puede costar deja de ser disponible para futuras órdenes. A partir de este momento la orden puede hacer match con otras
traded | La orden ha sido exitosamente transada
canceling | La orden ha recibido una petición de cancelación
canceled | La orden fue cancelada y el monto no transado está nuevamente disponible

### HTTP Request

`GET /markets/<market_id>/orders`

### Path Parameters

Parameter | Description
--------- | -----------
market_id | La ID del mercado (Ej: "btc-clp", "btc-cop")

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
per | 300 | Número de órdenes por página [min 1, max 300]
page | 1 | Número de página a recibir
state | None | Estado de la orden
minimun_exchanged | None | Minimo transado por la orden

### Response Details (JSON)

Key | Type | Description
--------- | --------- | ---------
id | [int] | ID de la orden
type | [string] | Dirección de la orden (Ej: "Bid", "Ask")
price_type | [string] | Tipo de orden (Ej: "limit", "market")
fee_currency | [currency] | Moneda en la cual es cobrada la tarifa (Ej: "BTC", "CLP" ,"COP")
limit | [amount_string, currency] | Precio de la orden
amount | [amount_string, currency] | Volumen pendiente de la orden
original_amount | [amount_string, currency] | Volumen original de la orden
traded_amount | [amount_string, currency] | Volumen transado de la orden
paid_fee | [amount_string, currency] | Tarifa pagada por la orden
total_exchanged | [amount_string, currency] | Total transado por la orden
total_pages | [int] | Cantidad total de páginas
total_count | [int] | Cantidad total de órdenes
current_page | [int] | Página actual

## Nueva Orden

> Example Payload:

```json
{
  "order": {
    "type": "Bid",
    "price_type": "limit",
    "limit": "230000.0",
    "amount": "0.4"
  }
}
```

> Example Code:

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.new_order("btc-clp", "bid", 0.05, 835875.00, "limit")
```

> The above command returns JSON structured like this:

```json
{
  "order": {
    "account_id": 358,
    "amount": ["0.05", "BTC"],
    "created_at": "2017-04-18T19:54:24.611Z",
    "fee_currency": "BTC",
    "id": 620196,
    "limit": ["835875.00", "CLP"]
    "market_id": 1,
    "original_amount": ["0.05", "BTC"],
    "paid_fee": ["0.0", "BTC"],
    "price_type": "limit",
    "state": "received",
    "total_exchanged": ["0.0", "CLP"],
    "traded_amount": ["0.0", "BTC"],
    "type": "Bid",
    "weighted_quotation": None
  }
}
```

Crea una nueva orden

### HTTP Request

`POST /markets/<market_id>/orders`

### Path Parameters

Parameter | Description
--------- | -----------
market_id | La ID del mercado (Ej: "btc-clp", "btc-cop")

### Request Payload

Key | Required | Description
--------- | ------- | -----------
type | Yes | Dirección de la orden (Ej: "Bid", "Ask")
price_type | Yes | Tipo de orden (Ej: "limit", "market")
limit | Limit | Precio de la orden
amount | Yes | Volumen de la orden

### Response Details (JSON)

Key | Type | Description
--------- | --------- | ---------
id | [int] | ID de la orden
type | [string] | Dirección de la orden (Ej: "Bid", "Ask")
price_type | [string] | Tipo de orden (Ej: "limit", "market")
fee_currency | [currency] | Moneda en la cual es cobrada la tarifa (Ej: "BTC", "CLP" ,"COP")
limit | [amount_string, currency] | Precio de la orden
amount | [amount_string, currency] | Volumen pendiente de la orden
original_amount | [amount_string, currency] | Volumen original de la orden
traded_amount | [amount_string, currency] | Volumen transado de la orden
paid_fee | [amount_string, currency] | Tarifa pagada por la orden
total_exchanged | [amount_string, currency] | Total transado por la orden
total_pages | [int] | Cantidad total de páginas
total_count | [int] | Cantidad total de órdenes
current_page | [int] | Página actual

<aside class="warning">
Advertencia — Asegurate de revisar bien tus cálculos antes de crear órdenes vía API!
</aside>

## Cancelar Orden

> Example Payload:

```json
{
  "state": "canceling"
}
```

> Example Code:

```python
from surbtc import SURBTC

order_id = 3

surbtc = SURBTC(api_key,api_secret,test)
surbtc.cancel_order(order_id)
```

> The above command returns JSON structured like this:

```json
{
  "order": {
    "account_id": 548,
    "amount": ["0.8317892", "BTC"],
    "created_at": "2017-04-18T16:09:38.089Z",
    "fee_currency": "CLP",
    "id": 620195,
    "limit": ["838305.78", "CLP"],
    "market_id": 1,
    "original_amount": ["0.8317892", "BTC"],
    "paid_fee": ["0.0", "CLP"],
    "price_type": "limit",
    "state": "canceling",
    "total_exchanged": ["0.0", "CLP"],
    "traded_amount": ["0.0", "BTC"],
    "type": "Ask",
    "weighted_quotation": None
  }
}
```

Permite comenzar la cancelación de una orden. No se puede realizar ningún otro cambio con este endpoint.

### HTTP Request

`PUT /orders/<id>`

### Path Parameters

Parameter | Description
--------- | -----------
id | La ID de la orden a cancelar

### Request Payload

Key | Required | Description
--------- | ------- | -----------
state | Yes | Debe indicar "canceling"

### Response Details (JSON)

Key | Type | Description
--------- | --------- | ---------
id | [int] | ID de la orden
type | [string] | Dirección de la orden (Ej: "Bid", "Ask")
price_type | [string] | Tipo de orden (Ej: "limit", "market")
fee_currency | [currency] | Moneda en la cual es cobrada la tarifa (Ej: "BTC", "CLP" ,"COP")
limit | [amount_string, currency] | Precio de la orden
amount | [amount_string, currency] | Volumen pendiente de la orden
original_amount | [amount_string, currency] | Volumen original de la orden
traded_amount | [amount_string, currency] | Volumen transado de la orden
paid_fee | [amount_string, currency] | Tarifa pagada por la orden
total_exchanged | [amount_string, currency] | Total transado por la orden
total_pages | [int] | Cantidad total de páginas
total_count | [int] | Cantidad total de órdenes
current_page | [int] | Página actual

<aside class="success">
Recuerde — identifique bien la Id de la orden a cancelar con las llamadas de Mis Órdenes
</aside>

## Estado de la orden

> Example Code:
```python
from surbtc import SURBTC

order_id = 3

surbtc = SURBTC(api_key,api_secret,test)
surbtc.single_order(order_id)
```

> The above command returns JSON structured like this:

```json
{
  "order": {
            "account_id": 548,
            "amount": ["0.8317892", "BTC"],
            "created_at": "2017-04-18T16:09:38.089Z",
            "fee_currency": "CLP",
            "id": 620195,
            "limit": ["838305.78", "CLP"],
            "market_id": 1,
            "original_amount": ["0.8317892", "BTC"],
            "paid_fee": ["0.0", "CLP"],
            "price_type": "limit",
            "state": "canceled",
            "total_exchanged": ["0.0", "CLP"],
            "traded_amount": ["0.0", "BTC"],
            "type": "Ask",
            "weighted_quotation": None
            }
}

```

Permite ver los detalles del estado actualde una orden

### HTTP Request

`GET /orders/<id>`

### Path Parameters

Parameter | Description
--------- | -----------
id | La ID de la orden a consultar

### Response Details (JSON)

Key | Type | Description
--------- | --------- | ---------
id | [int] | ID de la orden
type | [string] | Dirección de la orden (Ej: "Bid", "Ask")
price_type | [string] | Tipo de orden (Ej: "limit", "market")
fee_currency | [currency] | Moneda en la cual es cobrada la tarifa (Ej: "BTC", "CLP" ,"COP")
limit | [amount_string, currency] | Precio de la orden
amount | [amount_string, currency] | Volumen pendiente de la orden
original_amount | [amount_string, currency] | Volumen original de la orden
traded_amount | [amount_string, currency] | Volumen transado de la orden
paid_fee | [amount_string, currency] | Tarifa pagada por la orden
total_exchanged | [amount_string, currency] | Total transado por la orden
total_pages | [int] | Cantidad total de páginas
total_count | [int] | Cantidad total de órdenes
current_page | [int] | Página actual


## Historial de depositos/retiros

> Example Payload:

```json
{
  "state": "canceling"
}
```

> Example Code:

```ruby
require "kittn"

api = Kittn::APIClient.authorize!("meowmeowmeow")
api.kittens.get
```

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.deposit_history("BTC")
surbtc.withdrawal_history("BTC")
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require("kittn");

let api = kittn.authorize("meowmeowmeow");
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
{Withdrawal
  "deposits": [
    {
      "id": 1,
      "created_at": "2017-04-11 20:24:51 +0000",
      "updated_at": "2017-04-11 20:24:51 +0000",
      "amount": ["0.4","BTC"],
      "currency": "BTC",
      "state": "pending_confirmation",
      "deposit_data": {
        "type": "bitcoin_deposit_data",
        "address": "mo366JJaDU5B1hmnPygyjQVMbUKnBC7DsY",
        "tx_hash": "51eaf04f9dbbc1417dc97e789edd0c37ecda88bac490434e367ea81b71b7b015"
      }
    },
    {
      "id": 2,
      "created_at": "2017-04-11 22:34:11 +0000",
      "updated_at": "2017-04-11 23:22:51 +0000",
      "amount": ["3.45","BTC"],
      "currency": "BTC",
      "state": "confirmed",
      "deposit_data": {
        "type": "bitcoin_deposit_data",
        "address": "3GnLXPijNXSVUh4JQw9q5SbhYaL3S57Zp7",
        "tx_hash": "0608f49741a150fac8f72d2d143d6fd9ef36333b224e41f7bcdc159b40b9b165"
      }
    }
  ]
}

{
  "withdrawals": [
    {
      "id": 1,
      "created_at": "2017-04-11 20:24:51 +0000",
      "updated_at": "2017-04-11 20:24:51 +0000",
      "state": "pending_execution",
      "amount": ["0.35","BTC"],
      "currency": "BTC",
      "withdrawal_data": {
        "type": "bitcoin_withdrawal_data",
        "target_address": "mo366JJaDU5B1hmnPygyjQVMbUKnBC7DsY",
        "tx_hash": "51eaf04f9dbbc1417dc97e789edd0c37ecda88bac490434e367ea81b71b7b015"
      }
    },
    {
      "id": 2,
      "created_at": "2017-04-11 20:24:51 +0000",
      "updated_at": "2017-04-11 20:24:51 +0000",
      "state": "confirmed",
      "amount": ["0.4","BTC"],
      "currency": "BTC",
      "withdrawal_data": {
        "type": "bitcoin_withdrawal_data",
        "target_address": "mo366JJaDU5B1hmnPygyjQVMbUKnBC7DsY",
        "tx_hash": "51eaf04f9dbbc1417dc97e789edd0c37ecda88bac490434e367ea81b71b7b015"
      }
    }
  ]
}
```

Permite consultar el historial de depositos y retiros

Para el estado de un depósito o retiro, entregado por el atributo state, los posibles valores son:

State | Description
--------- | -----------
pending_confirmation | El depósito no ha sido confirmado
confirmed | El depósito fué aceptado y el monto abonado
rejected | El depósito fué rechazado
retained | El depósito ha sido retenido, posiblemente por alguna violación a los términos del servicio

### HTTP Request

`GET /currencies/<currency_code>/deposits`

`GET /currencies/<currency_code>/withdrawals`


### Path Parameters

Parameter | Description
--------- | -----------
currency_code | Acrónimo de la moneda a listar

### Response Details (JSON)

Key | Type | Description
--------- | --------- | ---------
id | [int] | ID del deposito/retiro
created_at | [time] | Fecha de creación de la solicitud
updated_at | [time] | Fecha del último update de la solicitud
amount | [amount_string, currency] | Cantidad asociada al depósito/retiro
currency | [currency] | Moneda asociada al depósito/retiro
state | [string] | Estado de la solicitud (Ej: "confirmed", "rejected")
deposit_data | [array] | Arreglo con detalles depentientes del tipo de consulta
type | [string] | Tipo de data entregada (Ej: "bitcoin_deposit_data","fiat_withdrawal_data")
address | [address] | Dirección del abono
target_address | [address] | Dirección de destino del retiro
transaction_hash | [string] | ID de la transacción


## Nuevo Retiro

> Example Payload:

```json
{
  "withdrawal": {
    "amount": 2.5,
    "currency": "BTC",
    "withdrawal_data": {
      "target_address": "mo366JJaDU5B1hmnPygyjQVMbUKnBC7DsY"
    }
  }
}
```

> Example Code:

```python
from surbtc import SURBTC

target_address = "mo366JJaDU5B1hmnPygyjQVMbUKnBC7DsY"
withdrawal_amount = 2.5

surbtc = SURBTC(api_key,api_secret,test)
surbtc.withdraw("BTC",target_address,withdrawal_amount)
```

> The above command returns JSON structured like this:

```json
{
  "withdrawal": {
    "id": 1,
    "created_at": "2017-04-11 20:24:51 +0000",
    "updated_at": "2017-04-11 20:24:51 +0000"
  }
}
```

Genera una solicitud de retiro para el monto y moneda seleccionadas

### HTTP Request

`POST /currencies/<currency_code>/withdrawals`

### Path Parameters

Parameter | Description
--------- | -----------
currency_code | Acrónimo de la moneda a retirar

### Response Details (JSON)

Key | Type | Description
--------- | --------- | ---------
id | [int] | ID del retiro
created_at | [time] | Fecha de creación de la solicitud
updated_at | [time] | Fecha del último update de la solicitud


## Depósito dinero fiat

> Example Payload:

```json
{
  "deposit": {
    "amount": ["250000.00","CLP"]
  }
}
```

> Example Code:

```python
from surbtc import SURBTC

currency = "CLP"
amount = ["250000.00","CLP"]

surbtc = SURBTC(api_key,api_secret,test)
surbtc.new_fiat_deposit(currency,amount)
```

> The above command returns JSON structured like this:

```json
{
  "deposit": {
    "id": 1,
    "created_at": "2017-04-11 22:02:56 +0000",
    "updated_at": "2017-04-11 22:02:56 +0000",
    "amount": ["250000.00","CLP"],
    "currency": "BTC",
    "state": "confirmed",
    "deposit_data": {
       *
    }
  }
```

Genera una nueva notificación de depósito
<aside class="notice">
***Atención:*** Este endpoint solo permite crear depósitos Fiat, para depositos de BTC referirse a la sección de [Depósito de Criptomonedas](./#dep-sito-criptomonedas)
</aside>

El proceso de depósito de un monto en Fiat tiene 2 etapas:

- Realizar la transferencia bancaria.
- Generar un nuevo depósito por el monto de la transferencia enterior.

### HTTP Request

`POST /currencies/<currency_code>/deposits`

### URL Parameters

Parameter | Description
--------- | -----------
currency_code | Acrónimo de la moneda a depositar

### Response Details (JSON)

Key | Type | Description
--------- | --------- | ---------
id | [int] | ID del deposito/retiro
created_at | [time] | Fecha de creación de la solicitud
updated_at | [time] | Fecha del último update de la solicitud
amount | [amount_string, currency] | Cantidad asociada al depósito
currency | [currency] | Moneda asociada al depósito
state | [string] | Estado de la solicitud (Ej: "confirmed", "rejected")
deposit_data | [array] | Arreglo con detalles depentientes del tipo de consulta

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Depósito criptomonedas

> Example Payload:

```json
{
  "receive_address": {
  }
}
```

> Example Code:

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.new_crypto_deposit("BTC")
```

> The above command returns JSON structured like this:

```json
{
  "receive_address": {
    "id": 1,
    "created_at": "2017-04-11 20:24:51 +0000",
    "updated_at": "2017-04-11 20:24:51 +0000",
    "address": "mo366JJaDU5B1hmnPygyjQVMbUKnBC7DsY",
    "used": false
  }
}
```

Permite generar una nueva address para abonar criptomonedas a SURBTC
<aside class="notice">
***Atención:*** Este endpoint solo permite generar direcciones para depositos de BTC, para abonar dinero fiat referirse a la sección de [Depósito dinero fiat](./#dep-sito-dinero-fiat)
</aside>

### HTTP Request

`POST /currencies/<currency_code>/receive_addresses`

### URL Parameters

Parameter | Description
--------- | -----------
currency_code | Acrónimo de la cryptomoneda a depositar

### Response Details (JSON)

Key | Type | Description
--------- | --------- | ---------
id | [int] | ID de la dirección asociada
created_at | [time] | Fecha de creación
updated_at | [time] | Fecha del último update
address | [address] | Dirección asignada para depositar
used | [bool] | Determina si la address fue usada con anterioridad