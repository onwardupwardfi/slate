# Errors

Our API returns standard HTTP success or error status codes. For errors, we will also include extra information about what went wrong encoded in the response as JSON. The various HTTP status codes we might return are listed below.

In general: Codes in the 2xx range indicate success. Codes in the 4xx range indicate an error that failed given the information provided (e.g., a required parameter was omitted). Codes in the 5xx range indicate an error with OneDB's servers (these are rare).


## HTTP Status Codes

Code | Title | Description
--------- | ------- | -----------
200 | OK | Everything worked as expected.
400 | Bad Request | The request was unacceptable, often due to missing a required parameter.
401 | Unauthorized | No valid API key provided
402 | Request Failed | The parameters were valid but the request failed.
404 | Not Found | The requested resource doesn't exist
409 | Conflict | The request conflicts with another request (perhaps due to using the same idempotent key).
429 | Too Many Requests | Too many requests hit the API too quickly. We recommend an exponential backoff of your requests.
500,502,503,504 | Server Errors | Something went wrong on Upward's end.

## Error types

Type | Description
--------- | ---------
api_connection |	Failure to connect to Upward's API.
api_error	| API errors cover any other type of problem (e.g., a temporary problem with our servers), and are extremely uncommon.
authentication_error |	Failure to properly authenticate yourself in the request.
invalid_request	| Invalid request errors arise when your request has invalid parameters.
rate_limit_exceeded	| Too many requests hit the API too quickly.
validation_error	| Unable to validate POST/PUT
not_found	| Resource was not found.
