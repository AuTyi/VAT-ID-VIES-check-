# VAT-ID-VIES-check-

VIES VAT number validation

You can verify the validity of a VAT number issued by any EU Member State by selecting that Member State from the drop-down menu provided, and entering the number to be validated.
http://ec.europa.eu/taxation_customs/vies/vatRequest.html

To check whether the customer is "VAT liable", or is VAT number is ok, the VAT number can be checked via an API directly with the EU.
More: http://ec.europa.eu/taxation_customs/vies/faq.html

The API (SOAP) requests and sends an XML. This means we need to send the request as XML to the API. Now the EU web service is available via SOAP (Simple Object Access Protocol). SOAP + XML requests used to be popular formats for data exchange.
But that's no reason to worry, because SOAP and CURL are more or less the same. Also with cURL we can address a SOAP interface with XML.

When we call the wsdl http://ec.europa.eu/taxation_customs/vies/checkVatService.wsdl of this Vat API in the browser, to be able to process our request and also what data will be returned as an answer.

![VIES wsdl](/images/VIES_wsdl.png)

## Scheme of a VAT check

1. Function name "checkVat"
2. Parameters that are needed in red also.
3. Values that are returned as a response in green. 

The function would look like this: checkVat (countryCode; vatNumber)
Now need to send this like XML in which we pass the parameters countryCode and vatNumber.

```xml
<s11:Envelope xmlns:s11='http://schemas.xmlsoap.org/soap/envelope/'>
  <s11:Body>
    <tns1:checkVat xmlns:tns1='urn:ec.europa.eu:taxud:vies:services:checkVat:types'>
      <tns1:countryCode>COUNTRYCODE</tns1:countryCode>
      <tns1:vatNumber>VATNUMBER</tns1:vatNumber>
    </tns1:checkVat>
  </s11:Body>
</s11:Envelope>
```
Replace COUNTRYCODE and VATNUMBER with the relevant variables.

But we send this query not to wsdl, but to this webaddress: http://ec.europa.eu/taxation_customs/vies/services/checkVatService

To check and ensure if the VAT ID is valid, we need the response of the API to be checked.
At the top of point 3, we see the parameters returned. There is the parameter "valid" of the data type boolean (true or false).

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body><checkVatResponse xmlns="urn:ec.europa.eu:taxud:vies:services:checkVat:types">
     <countryCode>LT</countryCode>
     <vatNumber>12345678</vatNumber>
     <requestDate>2019-10-31+01:00</requestDate>
     <valid>false</valid>
     <name>---</name>
     <address>---</address>
   </checkVatResponse>
</soap:Body></soap:Envelope>
```
If we check these parameters now, we know "true" = the VAT ID is valid or "false" the VAT ID is not valid.
So this rquest now an be easelly wrapper with anny codde, like PHP or Jscript.

You can also easeally cheked it's work or not with **Postman.**
