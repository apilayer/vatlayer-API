# Specifications & Overview

## API Access Key & Authentication

After signing up, every user is assigned a personal API Access Key - a unique "password" provided to access any of the API's data endpoints.

To authenticate with the vatlayer API, simply attach your `access_key` to your preferred Endpoint URL:

```url
http://apilayer.net/api/validate?access_key=YOUR_ACCESS_KEY  
```

You can [sign up for a free Access Key here](https://vatlayer.com/product).

## API Response

All vatlayer API data is returned in easily parseable JSON format. Find below an example API response using the `validate` endpoint to validate a VAT number.

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

## API Endpoints - Quick Overview

The vatlayer API offers 4 main data endpoints, all of which providing different kinds of services and starting out with the following Base URL:

```url
http://apilayer.net/api/
```

Take a look at the following two API Endpoints: 

**Endpoint 1: Simple VAT number validation**

```url
// "validate" - simple VAT number validation
http://apilayer.net/api/validate
    ? access_key = YOUR_ACCESS_KEY
    & vat_number = VAT_NUMBER
```

**Endpoint 2: EU VAT rate for single country**

```url
// "rate" - get EU VAT rate for a specific country - via country code  
http://apilayer.net/api/rate
    ? access_key = YOUR_ACCESS_KEY
    & country_code = GB
    
// "rate" - get EU VAT rate for a specific country - via custom IP address  
http://apilayer.net/api/rate
    ? access_key = YOUR_ACCESS_KEY
    & ip_address = 176.249.153.36
    
// "rate" - get EU VAT rate for a specific country - via client IP address  
http://apilayer.net/api/rate
    ? access_key = YOUR_ACCESS_KEY
    & use_client_ip = 1
```

**Endpoint 3: EU VAT rates for all countries**

```url
// "rate_list" - get VAT rates for all 28 EU countries
http://apilayer.net/api/rate_list
    ? access_key = YOUR_ACCESS_KEY
```

**Endpoint 4: Price calculation**

```url
// "rate_list" - convert prices in compliance with EU VAT rates
http://apilayer.net/api/price
    ? access_key = YOUR_ACCESS_KEY
    & amount = 100
    & country_code = GB
```

## 256-bit HTTPS Encryption

Paid Customers may establish a secure connection (industry-standard SSL) to the vatlayer API and all data provided by and accessible through it.

To connect securely, simply attach an `s` to the HTTP Protocol. (resulting in `https://`)


## API Error Codes

If your query fails, the screenshotlayer API will return a 3-digit error-code, an internal error type and a plain text "info" object containing suggestions for the user.

Find below an example error - triggered when the user did not provide a valid API Access Key:

```json
{
  "success": false,
  "error": {
    "code": 104,
    "type": "invalid_access_key",
    "info": "You have not supplied a valid API Access Key. [Technical Support: support@apilayer.net]"    
  }
}
```

The following list should help you find the most commonly returned API Errors:

| Type | Message  | Description |
|------------ |---------------| -----|
| 404 | "404_not_found" | User requested a resource which does not exist. |
| 101 | "missing_access_key" | User did not supply an Access Key. |
| 101 | "invalid_access_key" | User entered an invalid Access Key. |
| 103 | "invalid_api_function" | User requested a non-existend API Function. |
| 104 | "usage_limit_reached" | User has reached or exceeded his Subscription Plan's monthly API Request Allowance. |
| 210 | "no_vat_number_supplied" | User did not provide a VAT Number. |

[See all Error Types](https://vatlayer.com/documentation#error_codes)

## JSONP Callbacks

The vatlayer API also supports **JSONP** for cross-domain/AJAX requests. To use this feature, simply append:

`?callback=SOME_CALLBACK`

to any valid API Endpoint, and the result set will be returned **wrapped in the specified callback function**:

```json
http://apilayer.net/api/validate?callback=SOME_CALLBACK 
```

**Note**: The API also supports `Access-Control (CORS)` headers.

## JSON Formatting

In order to enhance readability (i.e. for de-bugging purposes), the currencylayer API features a built-in JSON `format` function, which displays the API's Response in typically JSON-structured format.

To enable this function, simply append `format=1` to any valid API Endpoint URL:

```
http://apilayer.net/api/validate
    ? access_key = YOUR_ACCESS_KEY
    [...]
    & format = 1      
```

Please be aware that enabling `format` increases the API Response's file size, which might have a negative on your application's performance.
