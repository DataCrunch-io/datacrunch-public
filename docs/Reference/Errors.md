# Errors

In addition to the standard HTTP status error codes, Error response data also contains the following keys:

Key | Info | 
---------|----------|
 code | Error type |
 message | Textual description of the error details |

 Error codes:


Code | Description | Matching HTTP error code
---------|----------|----------|
 invalid_request | Invalid request, usually an input error - missing or invalid property etc. | 400 |
 unauthorized_request | Access token is missing or invalid  | 401 |
 insufficient_funds | Not enough funds to perform the action | 402 |
 not_found | Resource not found | 404 |
 server_error | Error on datacrunch's side | 500 |
 service_unavailable | Not enough resources at the moment, try again later | 503 |