# Llamadas Privadas

## Account Info

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.info()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### URL Parameters

Parameter | Description
--------- | -----------
market_id | La ID del mercado (Ej: 'btc-clp', 'btc-cop')

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted

### Response Details

Key | Type | Description
--------- | --------- | ---------
asks | [amount, price] | Arreglo de ordenes del libro de ventas
bids | [amount, price] | Arreglo de ordenes del libro de compras


<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>


## Account Balance

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.balance()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
{
  "balance": {
    "id": "CLP",
    "amount": ["1000000.0","CLP"],
    "available_amount": ["800000.0","CLP"],
    "frozen_amount": ["100000.0","CLP"],
    "pending_withdraw_amount": ["100000.0","CLP"],
    "created_at": "2017-04-11 17:11:20 +0000",
    "updated_at": "2017-04-11 17:11:20 +0000"
  }
}
```

Muestra los balances de tu cuenta

### HTTP Request

`GET /balances`

`GET /balances/<currency>`

### URL Parameters

Parameter | Description
--------- | -----------
currency | *Opcional - El acrónimo de la mondea (Ej: 'btc', 'clp', 'cop')

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
Remember — a happy kitten is an authenticated kitten!
</aside>

## My Orders

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.orders('btc-clp')
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
{
  "orders": [
    {
      "id": 1,
      "type": "Bid",
      "price_type": "limit",
      "fee_currency": "BTC",
      "limit": ["230000","CLP"],
      "amount": ["0.4","BTC"],
      "original_amount": ["1.0","BTC"],
      "traded_amount": ["0.6","BTC"],
      "paid_fee": ["0.004","BTC"],
      "total_exchanged": ["130000","CLP"],
      "state": "pending",
      "created_at": "2017-04-11 17:11:20 +0000",
      "updated_at": "2017-04-11 17:11:20 +0000"
    },
    {
      "id": 2,
      "type": "Bid",
      "price_type": "limit",
      "fee_currency": "BTC",
      "limit": ["230000","CLP"],
      "amount": ["0.4","BTC"],
      "original_amount": ["1.0","BTC"],
      "traded_amount": ["0.6","BTC"],
      "paid_fee": ["0.004","BTC"],
      "total_exchanged": ["130000","CLP"],
      "state": "pending",
      "created_at": "2017-04-11 17:11:20 +0000",
      "updated_at": "2017-04-11 17:11:20 +0000"
    }
  ],
  "meta": {
    "total_pages": 1,
    "total_count": 2,
    "current_page": 1
  }
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

### URL Parameters

Parameter | Description
--------- | -----------
market_id | La ID del mercado (Ej: 'btc-clp', 'btc-cop')

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
type | [string] | Dirección de la orden (Ej: 'Bid', 'Ask')
price_type | [string] | Tipo de orden (Ej: 'limit', 'market')
fee_currency | [currency] | Moneda en la cual es cobrada la tarifa (Ej: 'BTC', 'CLP' ,'COP')
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
Remember — a happy kitten is an authenticated kitten!
</aside>

## New Order

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

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.orders('btc-clp')
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
{
  "order": {
    "id": 3,
    "type": "Bid",
    "price_type": "limit",
    "fee_currency": "BTC",
    "limit": ["230000","CLP"],
    "amount": ["0.5","BTC"],
    "original_amount": ["0.5","BTC"],
    "traded_amount": ["0.0","BTC"],
    "paid_fee": ["0.00","BTC"],
    "total_exchanged": ["0.0","CLP"],
    "state": "received",
    "created_at": "2017-04-11 20:21:39 +0000",
    "updated_at": "2017-04-11 20:21:39 +0000"
  }
}
```

Crea una nueva orden

### HTTP Request

`POST /markets/<market_id>/orders`

### URL Parameters

Parameter | Description
--------- | -----------
market_id | La ID del mercado (Ej: 'btc-clp', 'btc-cop')

### Request Payload

Key | Required | Description
--------- | ------- | -----------
type | Yes | Dirección de la orden (Ej: 'Bid', 'Ask')
price_type | Yes | Tipo de orden (Ej: 'limit', 'market')
limit | Limit | Precio de la orden
amount | Yes | Volumen de la orden

### Response Details (JSON)

Key | Type | Description
--------- | --------- | ---------
id | [int] | ID de la orden
type | [string] | Dirección de la orden (Ej: 'Bid', 'Ask')
price_type | [string] | Tipo de orden (Ej: 'limit', 'market')
fee_currency | [currency] | Moneda en la cual es cobrada la tarifa (Ej: 'BTC', 'CLP' ,'COP')
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
Remember — a happy kitten is an authenticated kitten!
</aside>

## Cancel Order

> Example Payload:

```json
{
  "state": "canceling"
}
```

> Example Code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
from surbtc import SURBTC

order_id = 3

surbtc = SURBTC(api_key,api_secret,test)
surbtc.cancel_order(order_id)
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
{
  "order": {
    "id": 3,
    "type": "Bid",
    "price_type": "limit",
    "fee_currency": "BTC",
    "limit": ["230000","CLP"],
    "amount": ["0.5","BTC"],
    "original_amount": ["0.5","BTC"],
    "traded_amount": ["0.0","BTC"],
    "paid_fee": ["0.00","BTC"],
    "total_exchanged": ["0.0","CLP"],
    "state": "canceling",
    "created_at": "2017-04-11 20:21:39 +0000",
    "updated_at": "2017-04-11 22:20:19 +0000"
  }
}
```

Permite comenzar la cancelación de una orden. No se puede realizar ningún otro cambio con este endpoint

### HTTP Request

`PUT /orders/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | La ID de la orden a cancelar

### Request Payload

Key | Required | Description
--------- | ------- | -----------
state | Yes | Debe indicar 'canceling'

### Response Details (JSON)

Key | Type | Description
--------- | --------- | ---------
id | [int] | ID de la orden
type | [string] | Dirección de la orden (Ej: 'Bid', 'Ask')
price_type | [string] | Tipo de orden (Ej: 'limit', 'market')
fee_currency | [currency] | Moneda en la cual es cobrada la tarifa (Ej: 'BTC', 'CLP' ,'COP')
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
Remember — a happy kitten is an authenticated kitten!
</aside>

## Order Status

> Example Code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
from surbtc import SURBTC

order_id = 3

surbtc = SURBTC(api_key,api_secret,test)
surbtc.order_satus(order_id)
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
{
  "order": {
    "id": 3,
    "type": "Bid",
    "price_type": "limit",
    "fee_currency": "BTC",
    "limit": ["230000","CLP"],
    "amount": ["0.5","BTC"],
    "original_amount": ["0.5","BTC"],
    "traded_amount": ["0.0","BTC"],
    "paid_fee": ["0.00","BTC"],
    "total_exchanged": ["0.0","CLP"],
    "state": "cancelled",
    "created_at": "2017-04-11 20:21:39 +0000",
    "updated_at": "2017-04-11 22:20:19 +0000"
  }
}
```

Permite ver los detalles del estado actualde una orden

### HTTP Request

`GET /orders/<id>`

### URL Parameters

Parameter | Description
--------- | -----------
id | La ID de la orden a consultar

### Response Details (JSON)

Key | Type | Description
--------- | --------- | ---------
id | [int] | ID de la orden
type | [string] | Dirección de la orden (Ej: 'Bid', 'Ask')
price_type | [string] | Tipo de orden (Ej: 'limit', 'market')
fee_currency | [currency] | Moneda en la cual es cobrada la tarifa (Ej: 'BTC', 'CLP' ,'COP')
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
Remember — a happy kitten is an authenticated kitten!
</aside>

## Deposit-Withdrawal History

> Example Payload:

```json
{
  "state": "canceling"
}
```

> Example Code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.deposit_history('BTC')
surbtc.withdrawal_history('BTC')
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
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


### URL Parameters

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
state | [string] | Estado de la solicitud (Ej: 'confirmed', 'rejected')
deposit_data | [array] | Arreglo con detalles depentientes del tipo de consulta
type | [string] | Tipo de data entregada (Ej: 'bitcoin_deposit_data','fiat_withdrawal_data')
address | [address] | Dirección del abono
target_address | [address] | Dirección de destino del retiro
transaction_hash | [string] | ID de la transacción

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## New Withdrawal

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

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
from surbtc import SURBTC

target_address = 'mo366JJaDU5B1hmnPygyjQVMbUKnBC7DsY'
withdrawal_amount = 2.5

surbtc = SURBTC(api_key,api_secret,test)
surbtc.withdraw('BTC',target_address,withdrawal_amount)
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
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

### URL Parameters

Parameter | Description
--------- | -----------
currency_code | Acrónimo de la moneda a retirar

### Response Details (JSON)

Key | Type | Description
--------- | --------- | ---------
id | [int] | ID del retiro
created_at | [time] | Fecha de creación de la solicitud
updated_at | [time] | Fecha del último update de la solicitud

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## New Fiat Deposit

> Example Payload:

```json
{
  "deposit": {
    "amount": ["250000.00","CLP"]
  }
}
```

> Example Code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
from surbtc import SURBTC

currency = 'CLP'
amount = ['250000.00','CLP']

surbtc = SURBTC(api_key,api_secret,test)
surbtc.new_fiat_deposit(currency,amount)
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
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

***Atención:*** Este endpoint solo permite crear depósitos Fiat, para depositos de BTC referirse a la sección de [New Crypto Deposit](./#new-crypto-deposit)

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
state | [string] | Estado de la solicitud (Ej: 'confirmed', 'rejected')
deposit_data | [array] | Arreglo con detalles depentientes del tipo de consulta

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## New Crypto Deposit

> Example Payload:

```json
{
  "receive_address": {
  }
}
```

> Example Code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.new_crypto_deposit('BTC')
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
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

Permite generar una nueva address para abonar cryptomonedas a SURBTC

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

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>
