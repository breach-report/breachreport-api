
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
    <a href=./04-monitor.md">
      Monitor
    </a>
    <span> | </span>
    <a href="./05-get-postback-url.md">
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
  Working with the Account
</h1>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>

Most Breach Report API calls are only applicable to the email addresses and web domains that have been registered with the consumer's account.

This chapter describes the following API calls:

* [Add an email address to the account](#add-an-email-address)
* [Add a web domain to the account](#add-a-domain-name)
* [Get the list of registered email addresses](#get-the-email-list)
* [Get the list of registered web domains](#get-the-domain-list)
* [Remove an email address from the account](#delete-an-email-address)
* [Remove a web domain from the account](#delete-a-domain)
* [Check a registered email address for data breach incidents](#check-a-registered-email-address)
* [Check a registered web domain for data breach incidents](#check-a-registered-domain)

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>

## Add an Email Address

**Request URL**: `{BASE_URL}/api/enterprise/v1/email/`

**Request method:** `POST`

This API call accepts an email address and adds it to a Breach Report account. The BR account must be associated with the secret API key (needs to be included in the request header).

The request returns a response code and a status message.

How to construct the request:

1. Include the API key in the request header.
2. Specify the email address in the request body.

### Request parameters

<details>
<summary>Show the parameters.</summary>
<br>

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | The key you can generate on the [Portal](https://breachreport.com/portal/user-api). Must be included in the request header. |
| email | string | Email address to be checked. |

</details>

### Code Examples

<details>
<summary>Shell code example.</summary>
<br>

```shell
curl --location --request POST '{{BASE_URL}}/api/enterprise/v1/email' \
--header 'api-key: {{API_KEY}}' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'email=me@vassily.pro'
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
var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: urlencoded,
  redirect: 'follow'
};

fetch("{{BASE_URL}}/api/enterprise/v1/email", requestOptions)
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
url = "{{BASE_URL}}/api/enterprise/v1/email"
payload = 'email=me@vassily.pro'
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

url = URI("{{BASE_URL}}/api/enterprise/v1/email")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["api-key"] = "{{API_KEY}}"
request["Content-Type"] = "application/x-www-form-urlencoded"
request.body = "email=me@vassily.pro"

response = http.request(request)
puts response.read_body
```

</details>

### Request Parameters

<details>
<summary>Show the parameters.</summary>
<br>

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | An API key you can generate on the [Portal](https://breachreport.com/portal/user-api). Include this key in the request header. |
| email | string | Email you want to add to the account for monitoring. |

</details>

### Response Examples

<details>
<summary>Successfully registred an email address.</summary>
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
| id | string | ID of the requested email address in the Breach Report database. |
| emailAddress | string | The requested email address. |

</details>

<details>
<summary>Cannot add a registered email address again.</summary>
<br>

```json
{
  "status": "error",
  "message": "Email address already exists"
}
```

</details>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>


## Add a Domain Name

**Request URL**: `{BASE_URL}/api/enterprise/v1/domain/`

**Request method:** `POST`

This API request adds an internet domain to a Breach Report account. The target BR account is identified by the API key from the request header.

Some of the popular internet domains (gmail.com, facebook.com and such) are included in the API stop list and cannot be added.

The request returns a response code and a status message.

How to construct the request:

1. Include the API key in the request header.
2. Specify the domain name in the request body.

### Request Parameters

<details>
<summary>Show the parameters.</summary>
<br>

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | The API key (generated on the [Portal](https://breachreport.com/portal/user-api)). Must be included in the request header. |
| domain | string | Domain to register. |

</details>

### Code Examples

<details>
<summary>Shell code example.</summary>
<br>

```shell
curl --location --request POST '{{BASE_URL}}/api/enterprise/v1/domain' \
--header 'api-key: {{API_KEY}}' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'domain=vassily.pro'
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
urlencoded.append("domain", "test.com");
var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: urlencoded,
  redirect: 'follow'
};

fetch("{{BASE_URL}}/api/enterprise/v1/domain", requestOptions)
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
url = "{{BASE_URL}}/api/enterprise/v1/domain"
payload = 'domain=vassily.pro'
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
url = URI("{{BASE_URL}}/api/enterprise/v1/domain")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["api-key"] = "{{API_KEY}}"
request["Content-Type"] = "application/x-www-form-urlencoded"
request.body = "domain=vassily.pro"
response = http.request(request)
puts response.read_body
```

</details>

### Response Examples

<details>
<summary>Added the domain successfully.</summary>
<br>

```json
{
  "status": "success",
  "domain": {
    "id": "5e751009aab5935e61ce6ddd",
    "domainName": "smith-example.com"
  }
}

```

| Name | Type | Description |
| ------ | ------ | ------ |
| id | string | Identifier of the domain. |
| domainName | string | The domain name. |

</details>

<details>
<summary>Cannot register the same domain multiple times.</summary>
<br>

```json
{
  "status": "error",
  "message": "Domain smith-example.com already exists."
}
```

</details>

<details>
<summary>Cannot register a domain that's on the stop list.</summary>
<br>

```json
{
  "status": "error",
  "message": "You cannot add this domain to the watchlist."
}
```

</details>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>

## Get the Email List

**Request URL**: `{BASE_URL}/api/enterprise/v1/email`

**Request method:** `GET`

This call returns the list of email addresses previously registered under the API key owner's account.  

To construct the request, include the API key in the request header.

### Request Parameters

<details>
<summary>Show the parameters.</summary>
<br>

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | An API key you can generate on the [Portal](https://breachreport.com/portal/user-api). Include this key in the request header. |

</details>

### Code Examples

<details>
<summary>Shell command example.</summary>
<br>

```shell
curl --location --request GET '{{BASE_URL}}/api/enterprise/v1/email' \
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
  method: 'GET',
  headers: myHeaders,
  redirect: 'follow'
};

fetch("{{BASE_URL}}/api/enterprise/v1/email", requestOptions)
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
url = "{{BASE_URL}}/api/enterprise/v1/email"
payload = {}
headers = {
  'api-key': '{{API_KEY}}'
}
response = requests.request("GET", url, headers=headers, data = payload)
print(response.text.encode('utf8'))
```

</details>

<details>
<summary>Ruby code example.</summary>
<br>

```ruby
require "uri"
require "net/http"
url = URI("{{BASE_URL}}/api/enterprise/v1/email")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Get.new(url)
request["api-key"] = "{{API_KEY}}"
response = http.request(request)
puts response.read_body
```

</details>

### Response Examples

<details>
<summary>Returned the email list.</summary>
<br>

```json
{
  "status": "success",
  "data": [
    {
      "id": "5e4e968511944f4cf3184eb3",
      "emailAddress": "test@test.com",
      "inWatchlist": true,
      "isAssignToDomain": false
    },
    {
      "id": "5e550fafaab5935e61ce6ddc",
      "emailAddress": "john.smith@example.com",
      "inWatchlist": false,
      "isAssignToDomain": false
    }
  ]
}

```

| Name | Type | Description |
| ------ | ------ | ------ |
| status | string | Operation status - `success` or `error`. |
| data | nested object | List / array of email address items. |
| id | string | Email address ID. |
| emailAddress | integer | Plaintext email address. |
| inWatchlist | boolean | This logical value shows whether the email address is currently being monitored (`true` or `false`). |
| isAssignToDomain | boolean | This value shows whether the email address is currently assigned to a domain. |

</details>

<details>
<summary>Sample response example.</summary>
<br>

</details>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>

## Get the Domain List

**Request URL**: `{BASE_URL}/api/enterprise/v1/domain`

**Request method:** `GET`

This call returns the list of web domains attributed to the API key owner's account.  

To construct the request, include the API key in the request header.

### Request Parameters

<details>
<summary>Show the parameters.</summary>
<br>

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | An API key you can generate on the [Portal](https://breachreport.com/portal/user-api). Must be included in the request header. |

</details>

### Code Examples

<details>
<summary>Shell command example.</summary>
<br>

```shell
curl --location --request GET '{{BASE_URL}}/api/enterprise/v1/domain' \
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
  method: 'GET',
  headers: myHeaders,
  redirect: 'follow'
};

fetch("{{BASE_URL}}/api/enterprise/v1/email", requestOptions)
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

Public
import requests
url = "{{BASE_URL}}/api/enterprise/v1/domain"
payload = {}
headers = {
  'api-key': '{{API_KEY}}'
}
response = requests.request("GET", url, headers=headers, data = payload)
print(response.text.encode('utf8'))
```

</details>

<details>
<summary>Ruby code example.</summary>
<br>

```ruby
require "uri"
require "net/http"

url = URI("{{BASE_URL}}/api/enterprise/v1/domain")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Get.new(url)
request["api-key"] = "{{API_KEY}}"

response = http.request(request)
puts response.read_body
```

</details>

### Response Examples

<details>
<summary>Returned a domain list.</summary>
<br>

```json
{
  "status": "success",
  "data": [
    {
      "id": "5e4e978211944f4cf3184eb4",
      "domainName": "vassily.pro",
      "inWatchlist": true,
      "emailList": [
        {
          "id": "5e4e9b0d11944f4cf3184eb7",
          "emailAddress": "me@vassily.pro"
        }
      ]
    },
    {
      "id": "5e551009aab5935e61ce6ddd",
      "domainName": "smith-example.com",
      "inWatchlist": false,
      "emailList": []
    }
  ]
}
```

| Name | Type | Description |
| ------ | ------ | ------ |
| status | string | Operation status - success or error. |
| data | integer | List / array of registered web domains. |
| domainName | integer | Domain name. |
| id | string | The domain's unique identifier in the Breach Report database. |
| inWatchlist | boolean | Indicates whether the domain is in the watchlist (in other words, is currently monitored). |
| emailList | nested object | Related email addresses, if any. |

</details>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>

## Delete an Email Address

**Request URL**: `{BASE_URL}/api/enterprise/v1/email/{EMAIL_ID}`

**Request method:** `DEL`

The API request accepts a previously added email address's ID from the Breach Report database and removes the associated email address from the account.

The request returns a response code and a status message.

How to construct the request:

1. Include the API key in the request header.
2. Specify your hashed email in the request body.

### Request Parameters

<details>
<summary>Show the parameters.</summary>
<br>

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | An API key you can generate on the [Portal](https://breachreport.com/portal/user-api). Include this key in the request header. |
| email | string | Email you want to check. |

</details>

### Code Examples

<details>
<summary>Shell command example.</summary>
<br>

```shell
curl --location --request DELETE '{{BASE_URL}}/api/enterprise/v1/email/5e4d65741eb6bb316c90fef2' \
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

fetch("{{BASE_URL}}/api/enterprise/v1/email/5e4d65741eb6bb316c90fef2", requestOptions)
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
url = "{{BASE_URL}}/api/enterprise/v1/email/5e4d65741eb6bb316c90fef2"
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
url = URI("{{BASE_URL}}/api/enterprise/v1/email/5e4d65741eb6bb316c90fef2")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Delete.new(url)
request["api-key"] = "{{API_KEY}}"
response = http.request(request)
puts response.read_body
```

</details>

### Response Examples

<details>
<summary>Successfully deleted the email address.</summary>
<br>

```json
{
  "status": "success",
  "email": {
    "id": "5e4e923611944f4cf3184eb5",
    "emailAddress": "john.smith@example.com"
  }
}

```

| Name | Type | Description |
| ------ | ------ | ------ |
| status | string | Operation result - `success` or `error`. |
| id | string | Email address ID in the Breach Report database. |
| emailAddress | boolean | Email is verified the user: True/False. |

</details>

<details>
<summary>Cannot delete an email address that's not registered.</summary>
<br>

```json
{
  "status": "error",
  "message": "Email with current id does not exist"
}
```

</details>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>

## Delete a Domain

**Request URL**: `{BASE_URL}/api/enterprise/v1/domain/{DOMAIN_ID}`

**Request method:** `DEL`

The API call accepts a previously added domain's ID and removes the associated domain entry from the account.

The request returns a response code and a status message.

How to construct the request:

1. Include the API key in the request header.
2. Specify the domain ID in the requested URL address.

### Request Parameters

<details>
<summary>Show the parameters.</summary>
<br>
</details>


### Code Examples

<details>
<summary>Shell command example.</summary>
<br>

```shell
curl --location --request DELETE '{{BASE_URL}}/api/enterprise/v1/domain/5e4d82332d313f32626f8481' \
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

fetch("{{BASE_URL}}/api/enterprise/v1/domain/5e4d82332d313f32626f8481", requestOptions)
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
url = "{{BASE_URL}}/api/enterprise/v1/domain/5e4d82332d313f32626f8481"
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
url = URI("{{BASE_URL}}/api/enterprise/v1/domain/5e4d82332d313f32626f8481")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Delete.new(url)
request["api-key"] = "{{API_KEY}}"
response = http.request(request)
puts response.read_body
```

</details>

### Response Examples

<details>
<summary>Removed the domain successfully.</summary>
<br>

```json
{
  "status": "success",
  "domain": {
    "id": "5e5643007450412325943862",
    "domainName": "test-user3.com"
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
<summary>Cannot remove an unregistered domain.</summary>
<br>

{
  "status": "error",
  "message": "Domain with current id does not exist"
}

</details>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>

## Check a Registered Email Address

**Request URL**: `{BASE_URL}/api/enterprise/v1/email/{EMAIL_ID}/check`

**Request method:** `GET`

The request accepts the email address ID and returns information on related data breaches.

Alternatively, you may check any email address (previously added or a new one) using a [hashed email address value](#check-a-hashed-email-address) (recommended method) or a [plaintext value](#check-a-plaintext-email-address).

This API call returns:

* Incidents count for **unverified** emails.
* Incidents count and details for **verified** emails.

How to construct the request:

1. Include the API key in the request header.
2. Specify the domain ID in the requested URL address.

### Request Parameters

<details>
<summary>Show the parameters.</summary>
<br>

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | An API key you can generate on the [Portal](https://breachreport.com/portal/user-api). Should be included in the request header. |
| email | string | Email you want to check. |

</details>

### Code Examples

<details>
<summary>Shell command example.</summary>
<br>

```shell
curl --location --request POST '{{BASE_URL}}/api/enterprise/v1/email/check' \
--header 'api-key: {{API_KEY}}' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'email=test@test.com'
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
  method: 'GET',
  headers: myHeaders,
  redirect: 'follow'
};

fetch("{{BASE_URL}}/api/enterprise/v1/email/5e454bbb575c76a755085afe/check", requestOptions)
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
url = "{{BASE_URL}}/api/enterprise/v1/email/check"
payload = 'email=test@test.com'
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
url = URI("{{BASE_URL}}/api/enterprise/v1/email/check")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["api-key"] = "{{API_KEY}}"
request["Content-Type"] = "application/x-www-form-urlencoded"
request.body = "email=test@test.com"
response = http.request(request)
puts response.read_body
```

</details>

### Response Examples

<details>
<summary>Found data breaches (verified email address).</summary>
<br>

{
  "email": "john.smith@example.com",
  "records": 36,
  "isAssigned": true,
  "inWatchlist": false,
  "breaches": [
    {
      "breachId": 7708,
      "title": "CouponMom.com",
      "createdAt": "2019-03-06T14:22:34.498Z",
      "compromisedAccounts": 11032696,
      "breachYear": 2014,
      "url": "https://www.couponmom.com/",
      "logo": "https://crm.breachreport.com/images/uploads/U3k2YeESa5BWQrCd.webp",
      "description": "In 2014, a file containing data from Coupon Mom website which was alegedly hacked was dumbed on the dark web. It includes 11M email addresses and plaintext passwords. Further investigation showed that the records correspond to the Armor Games breached database. The breach was announced as an exclusive leak to RaidForums website.",
      "breachDataTypes": [
        "email",
        "plaintext password"
      ]
    }
  ]
}

</details>

<details>
<summary>Found data breaches (unverified email address).</summary>
<br>

{
    "email": "test@example.com",
    "records": 34924,
    "isAssigned": false,
    "breaches": 3
}

</details>

<details>
<summary>Cannot find matches (incorrect Email ID).</summary>
<br>

```json
{
  "status": "error",
  "message": "Email does not exist"
}
```

</details>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>

## Check a Registred Domain

**Request URL**: `{BASE_URL}/api/enterprise/v1/domain/{DOMAIN_ID}/check`

**Request method:** `GET`

This API call accepts an API key and a domain ID, and returns information on previous data breaches for the domain.

Alternatively, you may check an email address by the [hashed email address](#hashed-email-check) (recommended method).

How to construct the request:

1. Include the API key in the request header.
2. Specify the email in the request body.

### Request Parameters

<details>
<summary>Show the parameters.</summary>
<br>

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | An API key you can generate on the [Portal](https://breachreport.com/portal/user-api). Should be included in the request header. |
| DOMAIN_ID | string | ID of the domain to check. |

</details>

### Code Examples

<details>
<summary>Shell command example.</summary>
<br>

```shell
curl --location --request GET '{{BASE_URL}}/api/enterprise/v1/domain/{{DOMAIN_ID}}/check' \
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
  method: 'GET',
  headers: myHeaders,
  redirect: 'follow'
};

fetch("{{BASE_URL}}/api/enterprise/v1/domain/5e454bbb575c76a755085afe/check", requestOptions)
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
url = "{{BASE_URL}}/api/enterprise/v1/domain/5e454bbb575c76a755085afe/check"
payload = {}
headers = {
  'api-key': '{{API_KEY}}'
}
response = requests.request("GET", url, headers=headers, data = payload)
print(response.text.encode('utf8'))
```

</details>

<details>
<summary>Ruby code example.</summary>
<br>

```ruby
require "uri"
require "net/http"
url = URI("{{BASE_URL}}/api/enterprise/v1/domain/5e454bbb575c76a755085afe/check")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Get.new(url)
request["api-key"] = "{{API_KEY}}"
response = http.request(request)
puts response.read_body
```

</details>

### Response Examples

<details>
<summary>Providing data breach information for the domain.</summary>
<br>

```json
{
  "domain": "string",
  "records": 0,
  "isAssigned": true,
  "breaches": [
    {
      "updatedAt": "string",
      "createdAt": "string",
      "title": "string",
      "breachId": "string",
      "compromisedAccounts": 0,
      "dataCompromised": "string",
      "description": "string",
      "url": "string",
      "breachMonth": 0,
      "breachYear": 0,
      "status": "string",
      "links": [
        "string"
      ],
      "cms": [
        "WordPress"
      ],
      "logo": "string"
    }
  ]
}
```

|Name|Type|Description|
|---|---|---|
|domain|path|string|true|Checked domain URL. |
|records|int|How many times an email was found in breach.|
|isAssigned|boolean|Whether an email is verified with the issuing account.|
|breaches|list|Array / list of breaches and their details.|
|updatedAt|DateTime|Breach update time.|
|createdAt|DateTime|Breach added to DB time.|
|encryption|string|Breach encryption.|
|title|string|Breach title.|
|breachId|int|An ID of a breach.|
|compromisedAccounts|int|Number of all compromised accounts.|
|dataCompromised|string|Compromised data types.|
|description|string|Description of the breach|
|url|string|The link to breach news source.|
|breachMonth|int|The month of a breach.|
|breachYear|int|The year of a breach.|
|status|string|Whether breach validity was verified by BR employees.|
|links | | |  
|cms | string |The CMS system the site runs on. Examples: `WordPress`, `Joomla` or `null`.|
|logo| string |Link to the breach logo. |

</details>

<details>
<summary>Cannot get a domain with this ID.</summary>
<br>

```json
{
  "status": "error",
  "message": "Domain does not exist"
}
```

</details>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>
