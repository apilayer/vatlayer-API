# Code Examples

## JavaScript (jQuery.ajax) - Validate VAT Number

```javascript
// set endpoint and your access key
var endpoint = 'validate'
var access_key = 'YOUR_ACCESS_KEY';
var vat_number = 'LU26375245';

// validate VAT number via AJAX call
$.ajax({
    url: 'https://apilayer.net/api/' + endpoint + '?access_key=' + access_key + '&vat_number=' + vat_number,   
    dataType: 'jsonp',
    success: function(json) {

    // Access and use your preferred validation result objects
    alert(json.valid);
    alert(json.query);
    alert(json.company_name);
    alert(json.company_address);
                
    }
});
```

**Please note:** In this example, the use of JSONP is entirely optional.
