# Code Examples

## PHP (cURL): Validate VAT Number:

```php
// set API Endpoint and Access Key
$endpoint = 'validate';
$access_key = 'YOUR_ACCESS_KEY';

// set VAT number
$vat_number = 'LU26375245';

// Initialize CURL:
$ch = curl_init('http://apilayer.net/api/'.$endpoint.'?access_key='.$access_key.'&vat_number='.$vat_number.'');  
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

// Store the data:
$json = curl_exec($ch);
curl_close($ch);

// Decode JSON response:
$validationResult = json_decode($json, true);

// Access and use your preferred validation result objects
$validationResult['valid'];
$validationResult['query'];
$validationResult['company_name'];
$validationResult['company_address'];
```

