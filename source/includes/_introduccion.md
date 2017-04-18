# Introducción

## Introducción

Ésta es la documentación de la API REST detrás de [SURBTC](https://www.surbtc.com). Luego de contactarnos, te proveeremos de la posibilidad de obtener y administrar api keys que permitirán que realices trading automático.

## Librerías Open Source

Esta es una recopilación de librerías de nuestra API en distintos lenguajes testeadas por el equipo de SURBTC.

Si deseas compartir tu librería con la comunidad ponte en contacto con nuestro equipo de soporte :-)

Lenguaje | Link
--------- | -----------
Python | [https://github.com/delta575/python-surbtc-api](https://github.com/delta575/python-surbtc-api)

## Autenticación

```ruby
OpenSSL::HMAC.hexdigest('sha384', key_secret, string_a_firmar)
```

```python
    def _sign_payload(self, method, path, params=None, payload=None):

        route = build_route(path, params)
        nonce = gen_nonce()

        if payload:
            j = json.dumps(payload).encode('utf-8')
            encoded_body = base64.standard_b64encode(j).decode('utf-8')
            string = method + ' ' + route + ' ' + encoded_body + ' ' + nonce
        else:
            string = method + ' ' + route + ' ' + nonce

         h = hmac.new(key=self.SECRET.encode('utf-8'),
                     msg=string.encode('utf-8'),
                     digestmod=hashlib.sha384)

        signature = h.hexdigest()

        return {
            'X-SBTC-APIKEY': self.KEY,
            'X-SBTC-NONCE': nonce,
            'X-SBTC-SIGNATURE': signature,
            'Content-Type': 'application/json',
        }
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```
> Another set of code.

Para autenticar una llamada [debes contactarnos](http://www.surbtc.com) para obtener una api key.

El api key provisto consistirá en un un key id y un key secret. 

SURBTC espera que todas sus llamadas privadas sean autenticadas siguiendo las siguientes instrucciones:

### 1. Generar un nonce
El nonce debe ser un número entero que debe siempre cumplir con la condición de ser mayor al último nonce usado. Una forma de lograr esto es usando un timestamp.

### 2. Preparar el string a firmar con el siguiente contenido:
En caso de GET:

`GET {ruta} {nonce}`

En caso de POST o PUT:

`{POST|PUT} {ruta} {base64_encoded_body} {nonce}`

Donde:

- <code>ruta</code>: Corresponde a la ruta completa del request incluyendo el query string pero sin el host. Ej: /api/v1/orders?open=true
- <code>base64_encoded_body</code>: Corresponde al contenido del request codificado en base 64.

### 3. Obtener la firma
La firma se obtiene aplicando la función de encriptación SHA-384 sobre el key secret y el string a firmar.


### 4. Adjuntar key id, nonce y firma en la cabecera del request
La firma debe ser adjuntada en formato hexadecimal.

Por ejemplo:

Header | Value
--------- | -----------
X-SBTC-APIKEY | 0faea2f360a508a6d105a3bb60247af0
X-SBTC-NONCE  | 145511231131231
X-SBTC-SIGNATURE  | 5c873eddb1117b930b1caa058ada3f7...


<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>
