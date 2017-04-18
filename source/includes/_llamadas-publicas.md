# Llamadas Públicas

## Ticker

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.ticker("btc-clp")
```

> The above command returns JSON structured like this:

```json
{
  "ticker": {
    "last_price": ["879789.0","CLP"],
    "max_bid": ["879658.0","CLP"],
    "min_ask": ["876531.11","CLP"],
    "price_variation_24h": 0.005,
    "price_variation_7d": 0.100,
    "volume": ["102.0","BTC"]
  }
}
```

El ticker permite ver el estado actual del mercado. Muestra la mejores ofertas de compra y venta (bid y ask), asi como el precio de la ultima transacción (last_price). También incluye información como el volumen diario y cuanto ha cambiado el precio en las últimas 24 hrs.

### HTTP Request

`GET /markets/<market_id>/ticker`

### Path Parameters

Parameter | Description
--------- | -----------
market_id | La ID del mercado (Ej: "btc-clp", "btc-cop")

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


## Order Book

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.order_book("btc-clp")
```

> The above command returns JSON structured like this:

```json
{
  "order_book": {"asks": [ ["836677.14", "0.447349"],
                           ["837462.23", "1.43804963"],
                           ["837571.89", "1.41498541"],
                           ["837597.23", "0.13177617"],
                           ["837753.25", "1.40724154"],
                           ["837858.51", "1.40988433"],
                           ["837937.0", "1.46619702"],
                           ["838000.57", "1.4527277"],
                           ["838305.78", "0.8317892"]
                         ],
                 "bids": [ ["821580.0", "0.25667389"],
                           ["821211.0", "0.27827307"],
                           ["819882.39", "1.40003128"],
                           ["819622.99", "1.40668862"],
                           ["819489.9", "1.41736995"],
                           ["818942.2", "1.41001753"],
                           ["818820.29", "0.93677863"],
                           ["816879.83", "1.44022295"]
                          ]
  }
}
```

Obtén el libro de órdenes completo

### HTTP Request

`GET /markets/<market_id>/order_book`

### Path Parameters

Parameter | Description
--------- | -----------
market_id | La ID del mercado (Ej: "btc-clp", "btc-cop")

### Response Details

Key | Type | Description
--------- | --------- | ---------
asks | [amount, price] | Arreglo de ordenes del libro de ventas
bids | [amount, price] | Arreglo de ordenes del libro de compras


## Trades

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.trades("btc-clp")
```

> The above command returns JSON structured like this:

```json
{
  "trades": [ [5689,"12-10-2017 02:58:32","-1.0","855441.15"],
              [5688,"12-10-2017 02:41:51","1.57698715","865485.0"],
              [5687,"12-10-2017 02:35:10","1.15486210","835415.0"],
              [5686,"12-10-2017 02:29:30","-4.0","851458.6"],
              [5685,"12-10-2017 02:26:24","3.25","865254.65"],
              [5684,"12-10-2017 02:18:22","-0.01","857054.45"],
              [5683,"12-10-2017 02:15:25","-1.011","863540.0"],
              [5682,"12-10-2017 02:08:32","12.55","865000.0"],
              [5681,"12-10-2017 02:07:45","0.1","852458.0"],
              [5680,"12-10-2017 02:03:37","1.01578414","845254.4"],
              [5679,"12-10-2017 01:58:52","1.84565103","852645.42"],
              [5678,"12-10-2017 01:58:01","-1.0","858687.0"],
              [5677,"12-10-2017 01:48:47","3.0","864521.0"],
              [5676,"12-10-2017 01:23:31","1.05","833582.0"],
              [5675,"12-10-2017 01:18:12","-5.56","836548.0"],
              [5674,"12-10-2017 01:11:32","-0.0535","835000.0"],
            ]
}
```

Obten una lista de las transacciones más recientes del mercado indicado

<aside class="warning">Falta endpoint</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### Path Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

### Response Details

Key | Type | Description
--------- | --------- | ---------
trades | [id, timestamp, amount, price] | Arreglo de transacciones


## Markets

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.markets()
```

> The above command returns JSON structured like this:

```json
{
  "markets": [
    {
      "base_currency": "BTC",
      "id": "BTC-CLP",
      "minimum_order_amount": ["0.001", "BTC"],
      "name": "btc-clp",
      "quote_currency": "CLP"
    },
    {
      "base_currency": "BTC",
      "id": "BTC-COP",
      "minimum_order_amount": ["0.001", "BTC"],
      "name": "btc-cop",
      "quote_currency": "COP"
    }
  ]
}

```

Un mercado permite separar las compras y ventas por moneda. En un mercado se compra y vende un tipo de moneda (base_currency) y se usa otro tipo de moneda para pagar por estas compras y ventas (quote_currency). Un mercado está identificado por estas dos monedas. Por ejemplo, el mercado que actualmente operamos permite vender y comprar bitcoins (BTC) usando pesos chilenos (CLP). Su identificador, por ende, es: btc-clp.

### HTTP Request

`GET /markets`

### Response Details

Key | Type | Description
--------- | --------- | ---------
markets | [array] | Arreglo de mercados disponibles
name | [market_id] | Nombre del mercado el cual corresponde al market_id
base_currency | [currency] | Moneda de cambio
quote_currency | [currency] | Moneda de pago