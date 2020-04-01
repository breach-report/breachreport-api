
<div align="center"><a name="menu"></a>
  <h4>
    <span> | </span>
    <a href="../README.md">
      General Info
    </a>
    <span> | </span>
    <a href="./01-before-using-api.md">
      Prepare
    </a>
    <span> | </span>
    <a href="./02-check-email-domains.md">
      Check Emails / Domains
    </a>
    <span> | </span>
    <a href="./03-manage-emails-domains.md">
      Manage Account
    </a>
    <span> | </span>
    <a href="./05-postback-url.md">
      Notifications
    </a>
    <span> | <span>
  </h4>
</div>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>

<h1 align="center">
  <a name="logo" href="http://breachreport.com"></a>
  <br>
  Monitoring Email Addresses and Web Domains
</h1>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>

Breach Report API enables the consumers to put email addresses and web domains on and off the watchlist.

The consumers will be alerted via the [Postback URL](), if the associated addresses and domains on the watchlist are subject to new data breach incidents.

This chapter describes the following calls:

* [Start email address monitoring](email-monitoring)
* [Start domain monitoring](domain-monitoring)
* [Stop email address monitoring](email-monitoring)
* [Stop domain monitoring](domain-monitoring)
* [Assign an email address to a domain](#assign-an-email-address)

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>

## Start Email Monitoring

**Request URL**: `{BASE_URL}/api/enterprise/v1/email/{EMAIL_ID}/monitoring`

**Request method:** `POST`

The API monitors the email addresses that are on the watchlist.

This API call accepts a previously added email addresses ID and puts the email address on the watchlist.

The request returns a response code and a status message.

How to construct the request:

1. Include the API key in the request header.
2. Specify your hashed email in the request body.

### Code Examples

```shell
curl --location --request POST '{{BASE_URL}}/api/enterprise/v1/email/5e4d82ab36839d32685374a6/monitoring' \
--header 'api-key: {{API_KEY}}'
```

```c
// Sample C code
```

```csharp
// Sample C# code
```


```go
// Sample Golang comments
```

```http
<!-- Sample HTTP code -->
```


```java
// Sample Java code
```

```javascript
// Fetch
```

```javascript
// NodeJS
```

```php
// Sample PHP code
```

```python
# Sample Python code - http.client
import http.client
import mimetypes
conn = http.client.HTTPSConnection("{{BASE_URL}}")
payload = ''
headers = {
  'api-key': '{{API_KEY}}'
}
conn.request("POST", "/api/enterprise/v1/email/5e4d82ab36839d32685374a6/monitoring", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))

# Sample Python code - requests
import requests
url = "{{BASE_URL}}/api/enterprise/v1/email/5e4d82ab36839d32685374a6/monitoring"
payload = {}
headers = {
  'api-key': '{{API_KEY}}'
}
response = requests.request("POST", url, headers=headers, data = payload)
print(response.text.encode('utf8'))
```

```ruby
require "uri"
require "net/http"
url = URI("{{BASE_URL}}/api/enterprise/v1/email/5e4d82ab36839d32685374a6/monitoring")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["api-key"] = "{{API_KEY}}"
response = http.request(request)
puts response.read_body
```

```swift
// Sample Swift Code
```


### Request Parameters

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | An API key you can generate on the [Portal](https://breachreport.com/portal/user-api). Include this key in the request header. |
| EMAIL_ID | string | Identifier of the previously added email address to be put on the watchlist.  |

> Email address successfully added to the watchlist.

### Response: Email address added to the watchlist

```json
{
  "status": "success",
  "email": {
    "id": "5e550fafaab5935e61ce6ddc",
    "emailAddress": "john.smith@example.com"
  }
}
```

| Name | Type | Description |
| ------ | ------ | ------ |
| status | string | Success or error. |
| id | string | Email address ID in the Breach Report database. |
| emailAddress | boolean | Email is verified the user: True/False. |

> Cannot add an email address to the watchlist again.

### Response: Email address has been added before

```json
{
  "status": "error",
  "message": "Email is already in monitoring list"
}
```


> Cannot find this Email ID.

### Response: Cannot find an email address with this ID

```json
{
  "status": "error",
  "message": "Email was not found. Please, add email and send request again."
}
```

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>


## Start Domain Monitoring

**Request URL**: `{BASE_URL}/api/enterprise/v1/email/{DOMAIN_ID}/monitoring`

**Request method:** `POST`

The API only monitors the domains that are on the watchlist.

This API call accepts a previously added domains's ID and puts the domain on the watchlist. The domain must be associated with the account.


How to construct the request:

1. Include the API key in the request header.
2. Specify the domain ID inside the requested URL.

### Code Examples

```shell
curl --location --request POST '{{BASE_URL}}/api/enterprise/v1/domain/5e4d82332d313f32626f8481/monitoring' \
--header 'api-key: {{API_KEY}}'
```


```javascript
// Fetch
```

```python
# Using requests
import requests
url = "{{BASE_URL}}/api/enterprise/v1/domain/5e4d82332d313f32626f8481/monitoring"
payload = {}
headers = {
  'api-key': '{{API_KEY}}'
}
response = requests.request("POST", url, headers=headers, data = payload)
print(response.text.encode('utf8'))
```

```ruby
require "uri"
require "net/http"
url = URI("{{BASE_URL}}/api/enterprise/v1/domain/5e4d82332d313f32626f8481/monitoring")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["api-key"] = "{{API_KEY}}"
response = http.request(request)
puts response.read_body
```



### Request Parameters

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | An API key you can generate on the [Portal](https://breachreport.com/portal/user-api). Include this key in the request header. |
| DOMAIN_ID | string | Identifier of the previously added domain to put on the watchlist.  |

> Domain has been successfully added  to the watchlist.

### Response: Domain successfully added to the watchlist

```json
{
  "status": "success",
  "domain": {
    "id": "5e562eac577e352dc7fae748",
    "domainName": "test-user.com"
  }
}
```

| Name | Type | Description |
| ------ | ------ | ------ |
| status | string | Success or error. |
| id | string | Domain identifier in the Breach Report database. |
| domainName | boolean | Domain name. |

> Cannot add a domain that's already on the watchlist.

### Response: Domain already on the watchlist

```json
{
  "status": "error",
  "message": "Domain already added to monitoring list"
}
```

> Cannot find this Domain ID.

### Response: Cannot find domain with this ID

After using the Domain ID that doesn't exist:

```json
{
  "status": "error",
  "message": "Domain was not found. Please, add email and send request again."
}
```
<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>


## Stop Email Monitoring

**Request URL**: `{BASE_URL}/api/enterprise/v1/email/{EMAIL_ID}/monitoring`

**Request method:** `DEL`

This API call accepts an email address ID, and removes the email address from the watchlist.  

How to construct the request:

1. Include the API key in the request header.
2. Specify the email address ID inside the requested URL.

### Code Examples

```shell
curl --location --request DELETE '{{BASE_URL}}/api/enterprise/v1/email/5e4d82ab36839d32685374a6/monitoring' \
--header 'api-key: {{API_KEY}}'
```

```python
# Sample Python code - http.client
import http.client
import mimetypes
conn = http.client.HTTPSConnection("{{BASE_URL}}")
payload = ''
headers = {
  'api-key': '{{API_KEY}}'
}
conn.request("DELETE", "/api/enterprise/v1/email/5e4d82ab36839d32685374a6/monitoring", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))

# Sample Python code - requests
import requests

url = "{{BASE_URL}}/api/enterprise/v1/email/5e4d82ab36839d32685374a6/monitoring"

payload = {}
headers = {
  'api-key': '{{API_KEY}}'
}

response = requests.request("DELETE", url, headers=headers, data = payload)

print(response.text.encode('utf8'))
```

```ruby
require "uri"
require "net/http"
url = URI("{{BASE_URL}}/api/enterprise/v1/email/5e4d82ab36839d32685374a6/monitoring")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Delete.new(url)
request["api-key"] = "{{API_KEY}}"
response = http.request(request)
puts response.read_body
```

<!-- ```c
// Sample C code
```

```csharp
// Sample C# code
```


```go
// Sample Golang comments
```

```http
Sample HTTP
```


```java
// Sample Java code
```

```javascript
// Fetch
```

```javascript
// NodeJS
```

```php
// Sample PHP code
```



```swift
// Sample Swift Code
``` -->


### Request Parameters

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | An API key you can generate on the [Portal](https://breachreport.com/portal/user-api). Include this key in the request header. |
| EMAIL_ID | string | Identifier of the previously added email address to put on the watchlist.  |

> The email address has been removed from the watchlist.

### Response: Email successfully removed from the watchlist

```json
{
  "status": "success",
  "email": {
    "id": "5e550fafaab5935e61ce6ddc",
    "emailAddress": "john.smith@example.com"
  }
}
```

| Name | Type | Description |
| ------ | ------ | ------ |
| status | string | Success or error. |
| id | string | Email address identifier in the Breach Report database. |
| emailAddress | boolean | Email address. |


> Cannot remove an email address that's on the watchlist.

### Response: Email address not on the watchlist

The example to the right shows the response when the requester attempts to remove an email address that's not on the watchlist.

```json
{
  "status": "error",
  "message": "Email does not exist in monitoring list"
}
```

> Cannot find this Email ID.

### Response: Cannot find an email address with this ID

```json
{
  "status": "error",
  "message": "Email was not found. Please, add email and send request again."
}
```

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>


## Stop Domain Monitoring

**Request URL**: `{BASE_URL}/api/enterprise/v1/domain/{DOMAIN_ID}/monitoring`

**Request method:** `DEL`

This API call accepts a domain ID and removes the domain from the watchlist. The domain must be associated with the account.

How to construct the request:

1. Include the API key in the request header.
2. Specify the domain ID inside the requested URL.

### Code Examples

```shell
curl --location --request DELETE '{{BASE_URL}}/api/enterprise/v1/domain/5e4d82332d313f32626f8481/monitoring' \
--header 'api-key: {{API_KEY}}'
```

```c
// Sample C code
```

```csharp
// Sample C# code
```

```go
// Sample Golang comments
```

```http
<p>Sample HTML Code</p>
```


```java
// Sample Java code
```

```javascript
// Fetch
```

```javascript
// NodeJS
```

```php
// Sample PHP code
```

```python
# Sample Python code - http.client
import http.client
import mimetypes
conn = http.client.HTTPSConnection("{{BASE_URL}}")
payload = ''
headers = {
  'api-key': '{{API_KEY}}'
}
conn.request("DELETE", "/api/enterprise/v1/domain/5e4d82332d313f32626f8481/monitoring", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))

# Sample Python code - requests
import requests
url = "{{BASE_URL}}/api/enterprise/v1/domain/5e4d82332d313f32626f8481/monitoring"
payload = {}
headers = {
  'api-key': '{{API_KEY}}'
}
response = requests.request("DELETE", url, headers=headers, data = payload)
print(response.text.encode('utf8'))
```

```ruby
require "uri"
require "net/http"
url = URI("{{BASE_URL}}/api/enterprise/v1/domain/5e4d82332d313f32626f8481/monitoring")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Delete.new(url)
request["api-key"] = "{{API_KEY}}"
response = http.request(request)
puts response.read_body
```

```swift
// Sample Swift Code
```

### Request parameters

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | An API key you can generate on the [Portal](https://breachreport.com/portal/user-api). Include this key in the request header. |
| DOMAIN_ID | string | Identifier of the previously added domain to put on the watchlist.  |

> Domain has been successfully removed from the watchlist.

### Response: Domain removed from the watchlist

```json
{
  "status": "success",
  "domain": {
    "id": "5e4f8cd511944f4cf3184eb9",
    "domainName": "test-example.com"
  }
}
```

| Name | Type | Description |
| ------ | ------ | ------ |
| status | string | Success or error. |
| id | string | Domain identifier in the Breach Report database. |
| domainName | boolean | Domain name. |


> Cannot remove a domain that is not on the watchlist.

### Response: Domain not on the watchlist, cannot be removed


```json
{
  "status": "error",
  "message": "Domain is not on the watchlist"
}
```

> Response example: Cannot remove a non-registered domain from the watchlist.

### Response: Cannot remove a non-registred domain

```json
{
  "status": "error",
  "message": "Domain was not found. Please, add domain and send request again."
}
```

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>


## Assign an Email Address

**Request method:** `POST`

**Request URL**: `{BASE_URL}/api/enterprise/v1/email/`

This API call accepts an API key, an email address, an internet domain ID, and assigns the email address to the domain. For the operation to be successful, both email address and domain must already exist in the API key owner's account.

The API call returns a response code and a status message.

Email address assigning enables the data to be aggregated across the domains registered to the account.

How to construct the request:

1. Include the API key in the request header.
2. In the request body, specify the email address and the Domain ID.


### Code Examples

```shell
curl --location --request POST '{{BASE_URL}}/api/enterprise/v1/email' \
--header 'api-key: {{API_KEY}}' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'email=me@vassily.pro' \
--data-urlencode 'domainId=5e4e978211944f4cf3184eb4'
```

```c
// Sample C code
```

```csharp
// Sample C# code
```

```go
// Sample Golang comments
```

```http
<!-- Sample HTTP code -->
```


```java
// Sample Java code
```

```javascript
// Fetch
```

```javascript
// NodeJS
```

```php
// Sample PHP code
```

```python
# Sample Python code - http.client
import http.client, mimetypes
conn = http.client.HTTPSConnection("{{BASE_URL}}")
payload = 'email=me@vassily.pro&domainId=5e4e978211944f4cf3184eb4'
headers = {
  'api-key': '{{API_KEY}}',
  'Content-Type': 'application/x-www-form-urlencoded'
}
conn.request("POST", "/api/enterprise/v1/email", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))

# Sample Python code - requests
import requests
url = "{{BASE_URL}}/api/enterprise/v1/email"
payload = 'email=me@vassily.pro&domainId=5e4e978211944f4cf3184eb4'
headers = {
  'api-key': '{{API_KEY}}',
  'Content-Type': 'application/x-www-form-urlencoded'
}
response = requests.request("POST", url, headers=headers, data = payload)
print(response.text.encode('utf8'))
```

```ruby
require "uri"
require "net/http"

url = URI("{{BASE_URL}}/api/enterprise/v1/email")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["api-key"] = "{{API_KEY}}"
request["Content-Type"] = "application/x-www-form-urlencoded"
request.body = "email=me@vassily.pro&domainId=5e4e978211944f4cf3184eb4"

response = http.request(request)
puts response.read_body
```

```swift
// Sample Swift Code
```

### Request parameters

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | An API key you can generate on the [Portal](https://breachreport.com/portal/user-api). Include this key in the request header. |
| email | string | Email address in the Breach Report database. |
| domainId | string | Identifier of the domain in the Breach Report database. |


> Email address has been assigned to the domain.

### Response: Email address successfully assigned

```json
{
  "status": "success",
  "email": {
    "id": "5e5626c8577e352dc7fae747",
    "emailAddress": "john@smith-example.com"
  }
}

```

| Name | Type | Description |
| ------ | ------ | ------ |
| id | string | ID of the domain the email address will be assigned to. |
| emailAddress | string | Email address to be assigned. |


> Email address was assigned before.

### Response: Email address already assigned

The email address has already been assigned.

```json
{
  "status": "error",
  "message": "Email address already exists"
}
```

> Target domain doesn't match the email domain

### Response: Email address doesn't match the domain

The target domain doesn't match the email address domain.

```json
{
  "status": "error",
  "message": "Email domain (example.com) is not equal to target domain (smith-example.com)"
}
```

> Response: Cannot find the email address

### Response: Cannot find the email address

```json
{
  "status": "error",
  "message": "Email address doesn't exist"
}
```

> Response: Cannot find the web domain.

### Response: Cannot find the domain

```json
{
  "status": "error",
  "message": "Domain doesn't exist"
}
```

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>
