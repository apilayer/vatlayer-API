# API Features

## VAT Number Validation

Both free and paid users may perform VAT number validations and company information lookups using the API's `validate` endpoint.

Simply specify the `vat_number` you would like to validate and append it to the API request URL.

**Example query:**

```url
http://apilayer.net/api/validate
    ? access_key = YOUR_ACCESS_KEY
    & vat_number = LU26375245   
```

**Example response:**

The API's response will consist of a number of different JSON objects containing information about your query, the respective company results and whether or not the validation was successful. Each object will be explained in the table below.

```json
{
  "valid": true,
  "format_valid": true,
  "query": "LU26375245",
  "country_code": "LU",
  "vat_number": "26375245",
  "company_name": "AMAZON EUROPE CORE S.A R.L.",
  "company_address": "5, RUE PLAETIS L-2338 LUXEMBOURG"
}          
```

**API response objects:**

| Object | Description |
| --------------|-------------- |
| `valid` | Contains `true` if the VAT number you entered could be successfully validated, `false` if not. |
| `format_valid` | Contains `true` if the syntax/format of the VAT number you entered is valid, `false` if not. |
| `query` | Returns the entire VAT number as per your API request. |
| `country_code` | The 2-letter country code assigned to the provided VAT number. |
| `vat_number` | The pure VAT number without the 2-letter country code prefix. |
| `company_name` | The name of the company the VAT number you entered is assigned to. |
| `company_address` | The address of the company the VAT number you entered is assigned to. |

## Get VAT Rates via Country Code

Using the vatlayer API's `rate` endpoint, you may request VAT rates for a country of your choice.

Just a quick **heads up**: There are 3 different parameters that can be used to define the country to obtain VAT rates for. In this example we'll define the country by appending the simple 2-letter `country_code` parameter to the request URL.

**Example query:**

```url
http://apilayer.net/api/rate
    ? access_key = YOUR_ACCESS_KEY
    & country_code = DE   
```

**Example response:**

Along with a `success` indication and country information, the API's JSON response will consist of a `standard_rate`, containing the specified country's standard VAT rate (in percent), and a `reduced_rates` object containing the country's VAT rate exceptions for specific types of goods.

```json
{
  "success": true,
  "country_code": "DE",
  "country_name": "Germany",
  "standard_rate": 19,
  "reduced_rates": {
    "foodstuffs": 7,
    "books": 7,
    "medical": 7,
    "passenger transport": 7,
    "newspapers": 7,
    "admission to cultural events": 7,
    "admission to entertainment events": 7,
    "hotels": 7
  }
}               
```

**Please note:** In case you provide a country code outside of the European Union, the vatlayer API will not return an error. Instead it will return a VAT rate of `0`.

## Reduced VAT Rates - Types of Goods

Each country declares individual types of goods that fall into reduced VAT categories, such as medical goods, hotels and books.

You may access a full list containing all available reduced VAT rate types via the API's `types` endpoint.

**Example query:**

```url
http://apilayer.net/api/types
    ? access_key = YOUR_ACCESS_KEY
```

**Please note:** Requesting the API's `types` endpoint does not count against your monthly API request volume.

## Get VAT Rates via IP Address

In the previous example we used the API's `rate` endpoint to request a single country's VAT rates via the country code. In this example we will specify the country by appending a custom `ip_address` as a query parameter.

**Example query:**

```url
http://apilayer.net/api/rate
    ? access_key = YOUR_ACCESS_KEY
    & ip_address = 134.245.44.17    
```

**Note**: In this case, the API response will be identical to the one listed in the previous example. ("Get VAT Rates via Country Code")

**Note**: In case you provide an IP address outside of the European Union, the vatlayer API will not return an error. Instead it will return a VAT rate of `0`.

## Get VAT Rates via Client IP

Additionally to EU VAT rates via the `country_code` and `ip_address` parameters, you may also request the API to define the country by using the very IP address the client is using the moment of calling the API.

To have the vatlayer API use the client's IP address as a country reference, simply set the API's `use_client_ip` parameter to `1`.

**Example query:**

```url
http://apilayer.net/api/rate
    ? access_key = YOUR_ACCESS_KEY
    & use_client_ip = 1      
```

**Note**: Except for the actual country returned, the API response will be the same as the one provided in the "Get VAT Rates via Country Code" example.

**Note**: In case the client's IP address is located outside of the European Union, the vatlayer API will not return an error. Instead it will return a VAT rate of 0.

## Retrieve all EU VAT Rates

Using the vatlayer API's `rate_list` endpoint, you may also request VAT rates for all 28 EU member states at once.

**Example query:**

```url
http://apilayer.net/api/rate_list
    ? access_key = YOUR_ACCESS_KEY
```

**Important:** Important: Please be aware that requesting an entire set of EU VAT rates results in a significantly larger JSON response size than when retrieving a single country's VAT rate, which may have a negative effect on your application's performance. It is highly recommended to request only one country's VAT information at once.

## VAT Compliant Price Calculation

Using the vatlayer API's `price` endpoint, you may request the API to calculate VAT compliant prices on your behalf. This API endpoint required two additional query parameters - explained below:

**Required parameter: conversion amount**

First, you'll need to specify the `amount` you would like to use for your VAT price calculation:

```bsh
amount         the amount/price you would like to calculate
               in compliance with EU VAT rates
```

**Required parameter: country**

As already mentioned above, there are three different parameters that can be used to define the country to obtain VAT rates for. Choose only one of the following:

```bsh
country_code     define country by appending a two 2-letter
                 country code of your choice
               
ip_address       define country by appending a custom IP
                 address to be located by the API
               
use_client_ip    define country by having the API locate the 
                 IP address of the client making the API call
```

**Example query:**

```url
http://apilayer.net/api/price
    ? access_key = YOUR_ACCESS_KEY
    & amount = 125
    & country_code = GB
```

**Please note:** As long as you don't specify an additional type parameter (see advanced options below), the API will automatically calculate your price using the respective country's standard VAT rate.

**Example response:**

The API response will confirm the country you provided by returing both a `country_code` and `country_name`. The amount you entered will be returned as `price_excl_vat`, and the VAT compliant price will be contained in the `price_incl_vat` object. Additionally, the vatlayer API will return a `vat_rate` object containint the EU VAT rate used for your price calculation.

```json
{
  "success": true,
  "country_code": "GB",
  "country_name": "United Kingdom",
  "price_excl_vat": 125,
  "price_incl_vat": 150,
  "vat_rate": 20
}        
```

### Advanced VAT Price Calculation Options

The `price` endpoint also offers two optional parameters:

**Optional parameter: type of goods**

In case your product falls under the "reduced VAT rates" category you may specify the type of goods according to the API's `types` endpoint.

```bsh
type      the type of goods according to the 
          API's "types" endpoint
```

**Optional parameter: VAT already included**

In case you are supplying an `amount` which already includes the respective VAT percentage and would like to request the API to perform a reverse calculation, simply set the API's incl parameter to `1`.

```bsh
incl      set to 1 if amount already includes 
          VAT percentage
```

**Example query:**

```url
http://apilayer.net/api/price
    ? access_key = YOUR_ACCESS_KEY
    & amount = 125
    & country_code = GB
    & type = medical
    & incl = 1
```

**Example response:**

In the United Kingdom there is no VAT rate applying to sales of `medical` products, therefore the API will return a `vat_rate` of `0`.

```json
{
  "success": true,
  "country_code": "GB",
  "country_name": "United Kingdom",
  "price_excl_vat": 125,
  "price_incl_vat": 125,
  "type": "medical"
  "vat_rate":0
}    
```



