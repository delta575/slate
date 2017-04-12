# Llamadas Públicas

## Ticker

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.ticker('btc-clp')
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
  "ticker": {
    "last_price": ["276000.0","CLP"],
    "min_ask": ["278000.0","CLP"],
    "max_bid": ["276000.0","CLP"],
    "volume": ["102.0","BTC"],
    "price_variation_24h": 0.05,
    "price_variation_7d": 0.1
  }
}
```

El ticker permite ver el estado actual del mercado. Muestra la mejores ofertas de compra y venta (bid y ask), asi como el precio de la ultima transacción (last_price). También incluye información como el volumen diario y cuanto ha cambiado el precio en las últimas 24 hrs.

### HTTP Request

`GET /markets/<market_id>/ticker`

### URL Parameters

Parameter | Description
--------- | -----------
market_id | La ID del mercado (Ej: 'btc-clp', 'btc-cop')

### Response Details

Key | Type | Description
--------- | --------- | ---------
last_price | [amount_string, currency] | Precio de la última orden ejecutada
min_ask | [amount_string, currency] | Menor precio de venta
max_bid | [amount_string, currency] | Máximo precio de compra
volume | [amount_string, currency] | Volumen transado en las últimas 24 horas
price_variation_24h | [float] | Cuanto ha variado el precio en las últimas 24 horas
price_variation_7d | [float] | Cuanto ha variado el precio el los últimos 7 días
timestamp * | [time] | Timestamp de la consulta


## Stats

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET /markets/<market_id>/ticker`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Order Book

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.order_book('btc-clp')
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "asks": [
    [
      "1.0",
      "200000.0"
    ],
    [
      "1.2",
      "201000.0"
    ],
    [
      "0.01",
      "202500.0"
    ],
    [
      "1.0",
      "210000.0"
    ]
  ],
  "bids": [
    [
      "0.5",
      "190000.0"
    ],
    [
      "0.01",
      "189000.0"
    ],
    [
      "1.0",
      "188900.0"
    ]
  ]
}
```

Obtén el libro de órdenes completo

### HTTP Request

`GET /markets/<market_id>/order_book`

### URL Parameters

Parameter | Description
--------- | -----------
market_id | La ID del mercado (Ej: 'btc-clp', 'btc-cop')

### Response Details

Key | Type | Description
--------- | --------- | ---------
asks | [amount, price] | Arreglo de ordenes del libro de ventas
bids | [amount, price] | Arreglo de ordenes del libro de compras


## Trades

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "asks": [
    [
      "1.0",
      "200000.0"
    ],
    [
      "1.2",
      "201000.0"
    ],
    [
      "0.01",
      "202500.0"
    ],
    [
      "1.0",
      "210000.0"
    ]
  ],
  "bids": [
    [
      "0.5",
      "190000.0"
    ],
    [
      "0.01",
      "189000.0"
    ],
    [
      "1.0",
      "188900.0"
    ]
  ]
}
```

Obten una lista de las transacciones más recientes del mercado indicado

<aside class="warning">Falta documentar</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

### Response Details

Key | Type | Description
--------- | --------- | ---------
asks | [amount, price] | Arreglo de ordenes del libro de ventas
bids | [amount, price] | Arreglo de ordenes del libro de compras

## Markets

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.markets()
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "markets": [
    {
      "name": "btc-clp",
      "base_currency": "btc",
      "quote_currency": "clp"
    },
    {
      "name": "btc-cop",
      "base_currency": "btc",
      "quote_currency": "cop"
    }
  ]
}
```

Un mercado permite separar las compras y ventas por moneda. En un mercado se compra y vende un tipo de moneda (base_currency) y se usa otro tipo de moneda para pagar por estas compras y ventas (quote_currency). Un mercado está identificado por estas dos monedas. Por ejemplo, el mercado que actualmente operamos permite vender y comprar bitcoins (BTC) usando pesos chilenos (CLP). Su identificador, por ende, es: btc-clp.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET /markets`

### Response Details

Key | Type | Description
--------- | --------- | ---------
markets | [array] | Arreglo de mercados disponibles
name | [market_id] | Nombre del mercado el cual corresponde al market_id
base_currency | [currency] | Moneda de cambio
quote_currency | [currency] | Moneda de pago


