# Errors

The InstantMerchant API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is not valid
401 | Unauthorized -- Your API key / secret is wrong
404 | Not Found -- The specified endpoint could not be found
405 | Method Not Allowed -- You tried to access a InstantMerchant with an invalid method
429 | Too Many Requests -- You're requesting too many API calls! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarially offline for maintanance. Please try again later.
