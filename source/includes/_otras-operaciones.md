# Otras Operaciones

## Buscar usuario por Id

> Example Payload:

```json
{
  "account_lookup": {
    "email": "contacto@surbtc.com"
  }
}
```

> Example Code:

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.new_crypto_deposit('BTC')
```

> The above command returns JSON structured like this:

```json
{
  "account_lookup": {
    "email": "contacto@surbtc.com",
    "account_exists": true,
    "account_id": 200101
  }
}
```

Permite encontrar el id de un usuario necesario para operaciones tales como remesas o traspasos.

### HTTP Request

`POST /account_lookups`

### Response Details (JSON)

Key | Type | Description
--------- | --------- | ---------
email | [string] | Correo electrónico a buscar 
account_exists | [bool] | Resultado de la busqueda, 'true' si el usuario existe, 'false' si no
account_id | [int] | Si el usuario existe este campo contiene su id para operaciones


## Crear nueva API Key

> Example Payload:

```json
{
  "api_key": {
    "name": "Mi primera clave",
    "expiration_time": "2016-10-10 00:00:00"
  }
}
```

> Example Code:

```python
from surbtc import SURBTC

surbtc = SURBTC(api_key,api_secret,test)
surbtc.new_apikey('Mi nueva llave','1235487548.5541')
```

> The above command returns JSON structured like this:

```json
{
  "api_key": {
    "name": "Mi primera clave",
    "expiration_time": "2016-10-10 00:00:00",
    "id": "0faea2f360a508a6d105a3bb60247af0",
    "secret": "ZWWOaA2w6u9OnUiLTQQ2pJVzqPX2UnsgxYCqcOro"
  }
}
```

Crea nueva API Key para llamadas autenticadas.

### HTTP Request

`POST /api_keys`

### Response Details (JSON)

Key | Type | Description
--------- | --------- | ---------
name | [string] | Nombre de la clave
expiration_time | [time] | Fecha de expiración de la clave (opcional)
id | [string] | Identificador de la clave, necesario para realizar la autenticación
secret | [string] | Secreto de la clave, necesario para firmar operaciones autenticadas

<aside class="warning">
Advertencia — Nunca expongas tus API Keys al público!
</aside>