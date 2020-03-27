# Configuring Postback URL Address

Breach Report API provides delivery of breach-related notifications to a user-specified URL address (Postback URL Address).

The Postback URL Address must start with `https://` prefix. 

The delivered updates provide information on discovered breaches for the domains and URL addresses that are on the watchlist.

You can manage the Postback URL value using the following operations:

* [Get current Postback URL Address value](#get-postback-url-address)
* [Set or update the Postback URL Address](#set-postback-url-address)
* [Remove the Postback URL Address altogether](#delete-postback-url-address)

<details>
<summary>Show the notification data example.</summary>
<br>

```json
{
 "emails": [
  {
   "id": "5e4d7b69d537e9321c432612",
   "emailAddress": "test@test.com",
   "breaches": [
    {
     "breachId": 1,
     "url": "http://Example.org",
     "title": "Example.com",
     "compromisedAccounts": 4540,
     "description": "In August 2019, breached files from a news platform Example.org surfaced on the web. The files included email accounts and passwords. The owners of the website have not made an official statement and the service was shut down shortly after hack. ",
     "breachMonth": 8,
     "breachYear": 2019,
     "breachDataTypes": [
      "email",
      "password"
     ]
    }
   ]
  }
 ],
 "domains": [
  {
   "id": "5e4ee6007c5549457f691cad",
   "domainName": "mydomain.com",
   "emails": [
    {
     "id": "5e4ee6267c5549457f691cae",
     "emailAddress": "test@mydomain.com",
     "breaches": [
      {
       "breachId": 3,
       "url": "https://www.mozilla.org/",
       "title": "MaliceSync.Bforce",
       "compromisedAccounts": 1694274,
       "description": "In April 2019, News Services issued an official statement notifying users on discovering a pattern of suspicious login attempts. Apparently attackers gained access to passwords that have been used on breached websites and attempted to login to user accounts.",
       "breachMonth": 3,
       "breachYear": 2019,
       "breachDataTypes": [
        "email",
        "password"
       ]
      }
     ]
    }
   ]
  }
 ]
}
```

</details>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>

## Get the Postback URL Address

**Request URL**: `{BASE_URL}/api/enterprise/v1/postback`

**Request method:** `GET`

Breach Report APi is designed to send notifications to the customer-specified URL address (postback URL). 

This call returns the current postback URL (if any) the API uses for notifications. 

How to construct the request:

1. Include the API key in the request header.
2. Specify your hashed email in the request body.

### Request Parameters

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | An API key you can generate on the [Portal](https://breachreport.com/portal/user-api). Include this key in the request header. |
| email | string | Email you want to check. |

### Code Examples

<details>
<summary>Shell command example.</summary>
<br>

```shell
curl --location --request GET '{{BASE_URL}}/api/enterprise/v1/postback' \
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

fetch("{{BASE_URL}}/api/enterprise/v1/postback", requestOptions)
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
url = "{{BASE_URL}}/api/enterprise/v1/postback"
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
url = URI("{{BASE_URL}}/api/enterprise/v1/postback")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Get.new(url)
request["api-key"] = "{{API_KEY}}"
response = http.request(request)
puts response.read_body
```

</details>



### Response Examples

<details>
<summary>Returning the current Postback URL value.</summary>
<br>

```json
{
  "status": "success",
  "postbackUrl": "https://example.com:3080/admin/user/test-webhook-url"
} 
```

| Name | Type | Description |
| ------ | ------ | ------ |
| status | string | Result of the operation - success or error. |
| postbackUrl | string | Current Postback URL address configuration. |

</details>

<details>
<summary>Cannot return the Postback URL when it's not configured.</summary>
<br>

```json
{
  "status": "success",
  "postbackUrl": null
}
```

</details>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>

## Set Postback URL Address

**Request URL**: `{BASE_URL}/api/enterprise/v1/postback`

**Request method:** `POST`

Breach Report APi is designed to send notifications to the customer-specified URL address (postback URL). 

This request accepts a URL address and sets it as a target for customer-focused Breach Report API notifications.  

The Postback URL Address must start with `https://` prefix. 

If a postback URL value was specified before, the call updates it with the new value. 

How to construct the request:

1. Include the API key in the request header.
2. Specify the postback URL in the request body.

### Request Parameters

<details>
<summary>Show the parameters.</summary>
<br>

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | An API key you can generate on the [Portal](https://breachreport.com/portal/user-api). Include this key in the request header. |
| url | string | Target postback URL address. |

</details>

### Code Examples

<details>
<summary>Shell command example.</summary>
<br>

```shell
curl --location --request POST '{{BASE_URL}}/api/enterprise/v1/postback' \
--header 'api-key: {{API_KEY}}' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'url=http://localhost:3080/admin/user/test-webhook-url'
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
urlencoded.append("url", "https://localhost/test-webhook-url");
var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: urlencoded,
  redirect: 'follow'
};

fetch("{{BASE_URL}}/api/enterprise/v1/postback", requestOptions)
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
url = "{{BASE_URL}}/api/enterprise/v1/postback"
payload = 'url=http%3A//localhost%3A3080/admin/user/test-webhook-url'
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
url = URI("{{BASE_URL}}/api/enterprise/v1/postback")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["api-key"] = "{{API_KEY}}"
request["Content-Type"] = "application/x-www-form-urlencoded"
request.body = "url=http%3A//localhost%3A3080/admin/user/test-webhook-url"
response = http.request(request)
puts response.read_body 
```

</details>


### Request Parameters

<details>
<summary>Has set the Postback URL successfully.</summary>
<br>

```json
{
  "status": "success",
  "postbackUrl": "https://example.com:3080/admin/user/test-webhook-url"
} 
```

| Name | Type | Description |
| ------ | ------ | ------ |
| status | string | Success or error. |
| postbackUrl| [string] | The list of credential types that were compromised in the incident. |


</details>



<details>
<summary>Cannot set a Postback URL address that is not in the correct format`.</summary>
<br>

```json
{
  "status": "error",
  "message": "Postback URL must started with https and be correct"
}
```

</details>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>

## Clear the Postback URL Address

**Request URL**: `{BASE_URL}/api/enterprise/v1/postback`

**Request method:** `DEL`

Breach Report APi is designed to send notifications to the customer-specified URL address (Postback URL address). 

The request clears the postback URL configuration from your account. As a result of the operation, the API will no longer be able to send you new notifications. 

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
curl --location --request DELETE '{{BASE_URL}}/api/enterprise/v1/postback' \
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

fetch("{{BASE_URL}}/api/enterprise/v1/postback", requestOptions)
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
url = "{{BASE_URL}}/api/enterprise/v1/postback"
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
url = URI("{{BASE_URL}}/api/enterprise/v1/postback")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Delete.new(url)
request["api-key"] = "{{API_KEY}}"
response = http.request(request)
puts response.read_body
```

</details>

### Response Examples

<details>
<summary>Cleared the Postback URL configuration successfully.</summary>
<br>

```json
{
  "status": "success"
} 
```

</details>

<details>
<summary>Cannot clear a Postback URL value that's not configured.</summary>
<br>

```json
{
  "status": "error",
  "message": "Postback urls does not exist"
}
```

</details>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>
