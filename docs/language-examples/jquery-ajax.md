# Code Examples

## JavaScript (jQuery.ajax) - Validate VAT Number

```javascript
// set endpoint and your access key
endpoint = 'validate'
access_key = 'YOUR_ACCESS_KEY';
vat_number = 'LU26375245';

// validate VAT number via AJAX call
$.ajax({
    url: 'http://apilayer.net/api/' + endpoint + '?access_key=' + access_key,   
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
