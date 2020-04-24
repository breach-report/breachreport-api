
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

If the associated addresses and domains on the watchlist are subject to new data breach incidents, the consumers will be alerted via the [Postback URL](./05-postback-url.md)

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

**Request URL**: `{BASE_URL}/api/v1/email/{EMAIL_ID}/monitoring`

**Request method:** `POST`

The API monitors the email addresses that are on the watchlist.

This API call accepts a previously added email addresses ID and puts the email address on the watchlist.

The request returns a response code and a status message.

How to construct the request:

1. Include the API key in the request header.
2. Specify your hashed email address in the request body.

### Request Parameters

<details>
<summary>Show the parameters.</summary>
<br>

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | An API key you can generate on the [Portal](https://breachreport.com/portal/user-api). Include this key in the request header. |
| EMAIL_ID | string | Identifier of the previously added email address to be put on the watchlist.  |

</details>

### Code Examples

<details>
<summary>Shell command example.</summary>
<br>

```shell
curl --location --request POST '{{BASE_URL}}/api/v1/email/5e4d82ab36839d32685374a6/monitoring' \
--header 'api-key: {{API_KEY}}'
```

</details>

<details>
<summary>JavaScript code example.</summary>
<br>

```javascript
// Using fetch()
var myHeaders = new Headers();
myHeaders.append("api-key", "{{API_KEY}}");
var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  redirect: 'follow'
};
fetch("{{BASE_URL}}/api/v1/domain/5e4d82332d313f32626f8481/monitoring", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

</details>

<details>
<summary>Python code example.</summary>
<br>

```python
# Using requests
import requests
url = "{{BASE_URL}}/api/v1/email/5e4d82ab36839d32685374a6/monitoring"
payload = {}
headers = {
  'api-key': '{{API_KEY}}'
}
response = requests.request("POST", url, headers=headers, data = payload)
print(response.text.encode('utf8'))
```

</details>

<details>
<summary>Ruby code example.</summary>
<br>

```ruby
require "uri"
require "net/http"
url = URI("{{BASE_URL}}/api/v1/email/5e4d82ab36839d32685374a6/monitoring")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["api-key"] = "{{API_KEY}}"
response = http.request(request)
puts response.read_body
```

</details>

### Response Examples

<details>
<summary>Added the email address to the watchlist successfully.</summary>
<br>

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

</details>

<details>
<summary>Cannot put a duplicate email address to the watchlist.</summary>
<br>

```json
{
  "status": "error",
  "message": "Email is already in monitoring list"
}
```

</details>

<details>
<summary>Cannot find an email address with this ID.</summary>
<br>

```json
{
  "status": "error",
  "message": "Email was not found. Please, add email and send request again."
}
```

</details>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>


## Start Domain Monitoring

**Request URL**: `{BASE_URL}/api/v1/email/{DOMAIN_ID}/monitoring`

**Request method:** `POST`

The API only monitors the domains that are on the watchlist.

This API call accepts a previously added domains's ID and puts the domain on the watchlist. The domain must be associated with the account.


How to construct the request:

1. Include the API key in the request header.
2. Specify the domain ID inside the requested URL. 

### Request Parameters

<details>
<summary>Show the parameters.</summary>
<br>

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | An API key you can generate on the [Portal](https://breachreport.com/portal/user-api). Include this key in the request header. |
| DOMAIN_ID | string | Identifier of the previously added domain to put on the watchlist.  |

</details>

### Code Examples

<details>
<summary>Shell command example.</summary>
<br>

```shell
curl --location --request POST '{{BASE_URL}}/api/v1/domain/5e4d82332d313f32626f8481/monitoring' \
--header 'api-key: {{API_KEY}}'
```

</details>

<details>
<summary>JavaScript code example.</summary>
<br>

```javascript
// Using fetch()
var myHeaders = new Headers();
myHeaders.append("api-key", "{{API_KEY}}");

var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  redirect: 'follow'
};

fetch("{{BASE_URL}}/api/v1/domain/5e4d82332d313f32626f8481/monitoring", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

</details>

<details>
<summary>Python code example.</summary>
<br>

```python
# Using requests
import requests
url = "{{BASE_URL}}/api/v1/domain/5e4d82332d313f32626f8481/monitoring"
payload = {}
headers = {
  'api-key': '{{API_KEY}}'
}
response = requests.request("POST", url, headers=headers, data = payload)
print(response.text.encode('utf8'))
```

</details>

<details>
<summary>Ruby code example.</summary>
<br>

```ruby
require "uri"
require "net/http"
url = URI("{{BASE_URL}}/api/v1/domain/5e4d82332d313f32626f8481/monitoring")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["api-key"] = "{{API_KEY}}"
response = http.request(request)
puts response.read_body
```

</details>

### Response Examples

<details>
<summary>Added the domain to the watchlist successfully.</summary>
<br>

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

</details>

<details>
<summary>Cannot add a duplicate domain to the watchlist.</summary>
<br>

```json
{
  "status": "error",
  "message": "Domain already added to monitoring list"
}
```

</details>

<details>
<summary>Cannot find a domain with this ID.</summary>
<br>

```json
{
  "status": "error",
  "message": "Domain was not found. Please, add email and send request again."
}
```

</details>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>


## Stop Email Monitoring

**Request URL**: `{BASE_URL}/api/v1/email/{EMAIL_ID}/monitoring`

**Request method:** `DEL`

This API call accepts an email address ID, and removes the email address from the watchlist.  

How to construct the request:

1. Include the API key in the request header.
2. Specify the email address ID inside the requested URL.

### Request Parameters

<details>
<summary>Show the parameters.</summary>
<br>

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | An API key you can generate on the [Portal](https://breachreport.com/portal/user-api). Include this key in the request header. |
| EMAIL_ID | string | Identifier of the previously added email address to put on the watchlist.  |

</details>

### Code Examples

<details>
<summary>Shell command example.</summary>
<br>

```shell
curl --location --request DELETE '{{BASE_URL}}/api/v1/email/5e4d82ab36839d32685374a6/monitoring' \
--header 'api-key: {{API_KEY}}'
```

</details>

<details>
<summary>JavaScript code example.</summary>
<br>

```javascript
// Using fetch()
var myHeaders = new Headers();
myHeaders.append("api-key", "{{API_KEY}}");
var requestOptions = {
  method: 'DELETE',
  headers: myHeaders,
  redirect: 'follow'
};
fetch("{{BASE_URL}}/api/v1/email/5e4d82ab36839d32685374a6/monitoring", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

</details>

<details>
<summary>Python code example.</summary>
<br>

```python
# Using requests
import requests
url = "{{BASE_URL}}/api/v1/email/5e4d82ab36839d32685374a6/monitoring"
payload = {}
headers = {
  'api-key': '{{API_KEY}}'
}
response = requests.request("DELETE", url, headers=headers, data = payload)
print(response.text.encode('utf8'))
```

</details>

<details>
<summary>Ruby code example.</summary>
<br>

```ruby
require "uri"
require "net/http"
url = URI("{{BASE_URL}}/api/v1/email/5e4d82ab36839d32685374a6/monitoring")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Delete.new(url)
request["api-key"] = "{{API_KEY}}"
response = http.request(request)
puts response.read_body
```

</details>


### Response Examples

<details>
<summary>Removed the email address from the watchlist successfully.</summary>
<br>

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

</details>

<details>
<summary>Cannot remove an email address that's not on the watchlist.</summary>
<br>

```json
{
  "status": "error",
  "message": "Email does not exist in monitoring list"
}
```

</details>

<details>
<summary>Cannot find an email address with this ID.</summary>
<br>

```json
{
  "status": "error",
  "message": "Email was not found. Please, add email and send request again."
}
```

</details>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>


## Stop Domain Monitoring

**Request URL**: `{BASE_URL}/api/v1/domain/{DOMAIN_ID}/monitoring`

**Request method:** `DEL`

This API call accepts a domain ID and removes the domain from the watchlist. The domain must be associated with the account.

How to construct the request:

1. Include the API key in the request header.
2. Specify the domain ID inside the requested URL.

### Request Parameters

<details>
<summary>Show the parameters.</summary>
<br>

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | An API key you can generate on the [Portal](https://breachreport.com/portal/user-api). Include this key in the request header. |
| DOMAIN_ID | string | Identifier of the previously added domain to put on the watchlist.  |

</details>

### Code Examples

<details>
<summary>Shell command example.</summary>
<br>

```shell
curl --location --request DELETE '{{BASE_URL}}/api/v1/domain/5e4d82332d313f32626f8481/monitoring' \
--header 'api-key: {{API_KEY}}'
```

</details>

<details>
<summary>JavaScript code example.</summary>
<br>

```javascript
// Using fetch()
var myHeaders = new Headers();
myHeaders.append("api-key", "{{API_KEY}}");
var requestOptions = {
  method: 'DELETE',
  headers: myHeaders,
  redirect: 'follow'
};
fetch("{{BASE_URL}}/api/v1/domain/5e4d82332d313f32626f8481/monitoring", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

</details>

<details>
<summary>Python code example.</summary>
<br>

```python
# Using requests
import requests
url = "{{BASE_URL}}/api/v1/domain/5e4d82332d313f32626f8481/monitoring"
payload = {}
headers = {
  'api-key': '{{API_KEY}}'
}
response = requests.request("DELETE", url, headers=headers, data = payload)
print(response.text.encode('utf8'))
```

</details>

<details>
<summary>Ruby code example.</summary>
<br>

```ruby
require "uri"
require "net/http"
url = URI("{{BASE_URL}}/api/v1/domain/5e4d82332d313f32626f8481/monitoring")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Delete.new(url)
request["api-key"] = "{{API_KEY}}"
response = http.request(request)
puts response.read_body
```

</details>


### Response Examples

<details>
<summary>Removed the domain from the watchlist successfully.</summary>
<br>

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

</details>




<details>
<summary>Cannot remove a domain that is not on the watchlist.</summary>
<br>

```json
{
  "status": "error",
  "message": "Domain is not on the watchlist"
}
```

</details>

<details>
<summary>Cannot find a domain with this ID.</summary>
<br>

```json
{
  "status": "error",
  "message": "Domain was not found. Please, add domain and send request again."
}
```

</details>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>


## Assign an Email Address

**Request method:** `POST`

**Request URL**: `{BASE_URL}/api/v1/email/`

This API call accepts an API key, an email address, an internet domain ID, and assigns the email address to the domain. For the operation to be successful, both email address and domain must already exist in the API key owner's account.

The API call returns a response code and a status message.

Email address assigning enables the data to be aggregated across the domains registered to the account.

How to construct the request:

1. Include the API key in the request header.
2. In the request body, specify the email address and the Domain ID.

### Request Parameters

<details>
<summary>Show the parameters.</summary>
<br>

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | An API key you can generate on the [Portal](https://breachreport.com/portal/user-api). Include this key in the request header. |
| email | string | Email address in the Breach Report database. |
| domainId | string | Identifier of the domain in the Breach Report database. |

</details>


### Code Examples

<details>
<summary>Shell command example.</summary>
<br>

```shell
curl --location --request POST '{{BASE_URL}}/api/v1/email' \
--header 'api-key: {{API_KEY}}' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'email=me@vassily.pro' \
--data-urlencode 'domainId=5e4e978211944f4cf3184eb4'
```

</details>

<details>
<summary>JavaScript code example.</summary>
<br>

```javascript
// Using fetch()
var myHeaders = new Headers();
myHeaders.append("api-key", "{{API_KEY}}");
myHeaders.append("Content-Type", "application/x-www-form-urlencoded");
var urlencoded = new URLSearchParams();
urlencoded.append("email", "test@test.com");
urlencoded.append("domainId", "5e4d82332d313f32626f8481");
var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: urlencoded,
  redirect: 'follow'
};
fetch("{{BASE_URL}}/api/v1/email", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

</details>

<details>
<summary>Python code example.</summary>
<br>

```python
# Using requests
import requests
url = "{{BASE_URL}}/api/v1/email"
payload = 'email=me@vassily.pro&domainId=5e4e978211944f4cf3184eb4'
headers = {
  'api-key': '{{API_KEY}}',
  'Content-Type': 'application/x-www-form-urlencoded'
}
response = requests.request("POST", url, headers=headers, data = payload)
print(response.text.encode('utf8'))
```

</details>

<details>
<summary>Ruby code example.</summary>
<br>

```ruby
require "uri"
require "net/http"
url = URI("{{BASE_URL}}/api/v1/email")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["api-key"] = "{{API_KEY}}"
request["Content-Type"] = "application/x-www-form-urlencoded"
request.body = "email=me@vassily.pro&domainId=5e4e978211944f4cf3184eb4"
response = http.request(request)
puts response.read_body
```

</details>


### Response Examples

<details>
<summary>Assigned the email address to domain successfully.</summary>
<br>

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

</details>

<details>
<summary>Cannot assign an email address that's already assigned.</summary>
<br>

```json
{
  "status": "error",
  "message": "Email address already exists"
}
```

</details>

<details>
<summary>Cannot assign an email address that doesn't match the domain.</summary>
<br>

```json
{
  "status": "error",
  "message": "Email domain (example.com) is not equal to target domain (smith-example.com)"
}
```

</details>


<details>
<summary>Cannot find this email address.</summary>
<br>

```json
{
  "status": "error",
  "message": "Email address doesn't exist"
}
```

</details>

<details>
<summary>Cannot find this web domain.</summary>
<br>

```json
{
  "status": "error",
  "message": "Domain doesn't exist"
}
```

</details>


<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>
