## Exchange Information

This section consists of endpoints that allows retrieval of Switcheo Exchange information.

Authentication is not required for these endpoints.

### GET Timestamp
Retrieve the current epoch [timestamp](#timestamp) in the exchange.

#### HTTP Request

`GET /v2/exchange/timestamp`

> Example response

```js
{
  "timestamp": 1534392760908
}
```

#### Notes

This value should be fetched and used when a timestamp parameter is required for API requests.

If the timestamp used for your API request is not within an acceptable range of the exchange's timestamp then an invalid signature error will be returned. The acceptable range might vary, but it should be less than one minute.

### GET Contracts

#### HTTP Request

`GET /v2/exchange/contracts`

> Example response

```js
{
    "NEO": {
        "V1": "<contract hash NEO 1>",
        "V1_5": "<contract hash NEO 1.5>",
        "V2": "<contract hash NEO 2>"
    },
    "ETH": {
        "V1": "<contract address ETH V1>"
    }
}
```

Returns the current deployed contract hashes by Switcheo Exchange.

Please note that a different set of contract hashes should be used
depending on the network you intend to interact with.

Network  | URL
-------- | ----------
TestNet  | Retrieve contract hashes from [TestNet_URL]/v2/exchange/contracts
MainNet  | Retrieve contract hashes from [MainNet_URL]/v2/exchange/contracts

ETH contract hashes should always include a `0x` prefix, while NEO contract hashes should never
includes a `0x` prefix.

### GET Latest Contracts

#### HTTP Request

`GET /v2/exchange/latest_contracts`

> Example response

```js
{
    "NEO": "d524fbb2f83f396368bc0183f5e543cae54ef532",
    "ETH": "0x6ee18298fd6bc2979df9d27569842435a7d55e65",
    "EOS": "oboluswitch4",
    "QTUM": "0x2b25406b0000c3661e9c88890690fd4b5c7b4234"
}
```

Returns the latest deployed contract hashes by Switcheo Exchange.

Please note that a different set of contract hashes should be used
depending on the network you intend to interact with.

Network  | URL
-------- | ----------
TestNet  | Retrieve contract hashes from [TestNet_URL]/v2/exchange/latest_contracts
MainNet  | Retrieve contract hashes from [MainNet_URL]/v2/exchange/latest_contracts

ETH contract hashes should always include a `0x` prefix, while NEO contract hashes should never
includes a `0x` prefix.

### GET Pairs

Retrieve available trading pairs on Switcheo Exchange filtered by the `base` parameter. Defaults to all pairs.

#### HTTP Request

`GET /v2/exchange/pairs`

> Example response without details

```js
// GET /v2/exchange/pairs?bases=["NEO"]&show_details=0
[
  "GAS_NEO",
  "SWTH_NEO",
  ...
]

```

> Example response with details

```js
// GET /v2/exchange/pairs?bases=["NEO"]&show_details=1
[
    {
        "name": "GAS_NEO",
        "price_precision": 3
    },
    {
        "name": "SWTH_NEO",
        "price_precision": 6,
    },
    ...
]

```

#### Request parameters

 Parameter              | Type            | Required  | Description
---------------         | -----------     | --------- | -----------
 bases                  | Array\<string\> | no        | Provides pairs for these base symbols.
 show_details           | Boolean         | no        | Show further details for token. Defaults to `0`


#### Response parameters for `show_details=0`

Parameter        | Description
------------     | ----------
Array            | List of token pairings (ticker) `<QUOTE>_<BASE>` where `<QUOTE>` is the trading token symbol (e.g. `SWTH`), while `<BASE>` is the base token symbol (e.g. `NEO`)

#### Response parameters for `show_details=1`

Parameter        | Description
------------     | ----------
name             | The ticker name as `<QUOTE>_<BASE>` where `<QUOTE>` is the trading token symbol (e.g. `SWTH`), while `<BASE>` is the base token symbol (e.g. `NEO`)
precision        | The maximum price precision that can be submitted (e.g. if precision is `2`, an order on this pair for `1.99` or `1.90000000` price is valid, but `1.999` is not)

### GET Tokens

Retrieve a list of supported tokens on Switcheo.

#### HTTP Request

`GET /v2/exchange/tokens`

> Example response

```js
// GET /v2/exchange/tokens?show_listing_details=1&show_inactive=1
{
  "NEO": {
        "hash": "c56f33fc6ecfcd0c225c4ab356fee59390af8560be0e930faebe74a6daff7c9b",
        "decimals": 8,
        "precision": 3,
        "trading_active": true
        "active": true,
        "listing_info": {
            "deposits": {
                "start": 1514764800,
                "end": 7983792000,
                "paused": false
            },
            "trading": {
                "start": 1514764800,
                "end": 7983792000,
                "paused": false
            },
            "cancellations": {
                "start": 1514764800,
                "end": 7983792000,
                "paused": false
            },
            "withdrawals": {
                "start": 1514764800,
                "end": 7983792000,
                "paused": false
            }
        }

 },
  ...
  "SWC": {
        "hash": "0x00bb907302e508707108cd835621dfd2b44ca7cf",
        "decimals": 18,
        "precision": 1,
        "trading_active": true
        "active": true,
        "listing_info": {
            "deposits": {
                "start": 1514764800,
                "end": 7983792000,
                "paused": false
            },
            "trading": {
                "start": 1514764800,
                "end": 7983792000,
                "paused": false
            },
            "cancellations": {
                "start": 1514764800,
                "end": 7983792000,
                "paused": false
            },
            "withdrawals": {
                "start": 1514764800,
                "end": 7983792000,
                "paused": false
            }
        }
   }
  ...
}
```

#### Request parameters

 Parameter              | Type      | Required  | Description
---------------         | --------- | --------- | -----------
 show_listing_details   | Boolean   | no        | Show all details for each token. If `false`, only `hash` & `decimals` are returned, otherwise all parameters below are returned. Defaults to `false`
 show_inactive          | Boolean   | no        | Show inactive tokens. Default to `false`)

#### Response parameters

Parameter         | Description
----------------- | ----------
hash              | Contract hash of the token
decimals          | Number of decimal places to use when submitting order amounts (e.g. if a token has `8` `decimals`, then an order for `1.2` tokens should be submitted as `120000000`)
precision         | The maximum amount precision that can be submitted in orders (e.g. if a token has `8` `decimals` and `2` precision, `123000000` would be a valid order amount to submit, but not `12340000`)
minimum_quantity  | The minimum order amount for any orders submitted involving this token. Note that this applies to both the order `want_amount` and `offer_amount` (or `quantity`, if using new format).
trading_active    | Whether this token is currently actively trading
active            | Whether this token is currently visible on the Switcheo Exchange UI
listing_info      | Status of the token for trading activities `["deposits", "trading", "cancellations", "withdrawals"]`  (only returned if `show_listing_details` is truthy)
start             | Starting time for the activity (in epoch seconds)
end               | Ending time for the activity (in epoch seconds)
paused            | Whether the trading of the token is temporarily paused

### GET Fees

> Example response

```js
[
  {
    "maker": {
      "default": 0
    },
    "taker": {
      "default": 0.002
    },
    "network_fee_subsidy_threshold": {
      "neo": 0.1,
      "eth": 0.1
    },
    "max_taker_fee_ratio": {
      "neo": 0.005,
      "eth": 0.1
    },
    "native_fee_discount": 0.75,
    "native_fee_asset_id": "ab38352559b8b203bde5fddfa0b07d8b2525e132",
    "enforce_native_fees": [
      "RHT",
      "RHTC"
    ],
    "native_fee_exchange_rates": {
        "NEO": "952.38095238",
        "GAS": "297.14285714",
        ...
        "ETH": "0",
        "JRC": "0",
        "SWC": "0"
    },
    "network_fees": {
      "eth": "2466000000000000",
      "neo": "200000"
    },
    "network_fees_for_wdl": {
      "eth": "873000000000000",
      "neo": "0"
    }
  }
]

```

Returns fee data for various blockchains and pairs

#### HTTP Request

`GET /v2/fees`

#### Response parameters

Parameter                 | Description
------------------------- | ----------
maker / default           | Default trading fee rate for maker orders, before any discount and network fees
taker / default           | Default trading rate for taker orders, before any discount and network fees
network_fee_subsidy_threshold | The rate used against your trade proceeds at which network fees will be capped at. (E.g. Assuming `network_fee_subsidy_threshold` is 0.1 and the your total trade proceeds is 0.2 ETH, the payable network fee will not exceed 0.02 ETH.)
native_fee_discount       | Discount applied to fee for use of native token (as a multiplier). Discount Rate is 1 - `native_fee_discount`
native_fee_asset_id       | NEO contract hash of the Native Token (i.e. `SWTH`)
enforce_native_fee        | List of asset tickers where fee **must** be paid in Native Tokens
native_fee_exchange_rates | List of token:value tuples, containing the exchange rate in Native Token
network_fees              | Network Trading fee in the smallest denomination of the native network token
network_fees_for_wdl      | Network Withdrawal fee in the smallest denomination of the native network token

### GET Announcements

Retrieve the currently active Switcheo Exchange Announcement

#### HTTP Request

`GET /v2/exchange/announcement_message`

> Example response

```js
{
  "messages": [
    {
      "message": "<span>Welcome onboard to <a href=\"https://medium.com/switcheo/callisto-is-now-live-de940a62f1a4\" target=\\\"blank\\\">Switcheo</a>, the first decentralized exchange on Ethereum and NEO.</span>",
      "message_type": "alert"
    },
    {
      "message": "<span>To access the legacy UI, please visit: <a href=\"https://legacy.switcheo.exchange\" target=\\\"blank\\\">https://legacy.switcheo.exchange/</a></span>",
      "message_type": "info"
    }
  ],
  "updated_at": "20181203"
}
```

#### Response parameters

Parameter    | Description
------------ | ----------
messages     | A list of messages
updated_at   | Last updated date in `YYYYMMDD` format


### GET Swap Pairs

Retrieve available swap pairs on Switcheo Exchange.

#### HTTP Request

`GET /v2/exchange/swap_pairs`

> Example response

```js
// GET /v2/exchange/swap_pairs
[
  "SWTH_ETH",
  ...
]

```

### GET Swap Pricing

Atomic Swap orders can be submitted using the [create order](#post-create-order) endpoint.
The amount that will be received for an offer amount is calculated by the Constant Product formula:

`x_receive = x - k / (y + y_give)`

+ x is the amount of asset X in the liquidity pool
+ y is the amount of asset Y in the liquidity pool
+ x_receive is the amount of asset X that the order submitter will receive
+ y_give is the amount of asset Y that the order submitter wants to give
+ k is x * y

For example, if the pair is SWTH / ETH, and the amount of SWTH in the liquidity pool is 3.6 million SWTH,
and the amount of ETH in the liquidity pool is 100 ETH, then for buying SWTH with 1 ETH, the order submitter would receive 35643.56436 SWTH:

`3.6 million - 360 million / (100 + 1) = 35643.56436 SWTH`

The values of x and y can be fetched from the swap pricing endpoint.

#### HTTP Request

`GET /v2/exchange/swap_pricing`

> Example response

```js
// GET /v2/exchange/swap_pricing?pair=SWTH_ETH
{
    "buy": {
        "x": "449293223000000",
        "y": "122561935387988990000",
        "k": "55066246967587328805614770000000000"
    },
    "sell": {
        "x": "122561935387988990000",
        "y": "449293223000000",
        "k": "55066246967587328805614770000000000"
    }
}
```

#### Request parameters

 Parameter   | Type      | Required  | Description
------------ | --------- | --------- | -----------
 pair        | String    | yes       | The [swap pair](#get-swap-pairs) to get the swap pricing of

### GET Atomic Swap Contracts

#### HTTP Request

`GET /v2/exchange/atomic_swap_contracts`

> Example response

```js
{
    "ETH": {
        "V1": "<contract hash>"
    }
}
```

Returns the current atomic swap contract hashes.

Please note that a different set of contract hashes should be used
depending on the network you intend to interact with.

Network  | URL
-------- | ----------
TestNet  | Retrieve contract hashes from [TestNet_URL]/v2/exchange/atomic_swap_contracts
MainNet  | Retrieve contract hashes from [MainNet_URL]/v2/exchange/atomic_swap_contracts
