
<div align="center"><a name="menu"></a>
  <h4>
    <span> | </span>
    <a href="../README.md">
      General Info
    </a>
    <span> | </span>
    <a href="./01-before-using-api.md">
      Before You Start
    </a>
    <span> | </span>
    <a href="./03-manage-emails-domains.md">
      Manage Account
    </a>
    <span> | </span>
    <a href="./04-monitor.md">
      Monitor
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
  Checking Domains and Email Addresses for Breaches
</h1>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>

Breach Report API enables the user to check email addresses for data breach incidents by providing the following calls:

* [Check a plaintext email address](#check-a-plaintext-email-address)
* [Check a hashed email address](#check-a-hashed-email-address)
* [Check a web domain for compromised email addresses](#check-a-web-domain)


<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>

# Check a Plaintext Email Address

**Request URL**: `{BASE_URL}/api/v1/email/check`

**Request method:** `POST`

This API call accepts a plaintext email address and checks it for known data breaches.

Alternatively, you may check an email address by using a [hashed email address](#check-a-hashed-email-address) (recommended method).

This API call returns:

* Incident count for **unverified** emails.
* Incident count and **details** for **verified** emails.

Verified email addresses are those that are confirmed by the API users via their email addresses.

How to construct the request:

1. Include the API key in the request header.
2. Specify the email address in the request body.

### Request parameters

<details>
<summary>Show the parameters.</summary>
<br>

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | The key you can generate on the [portal](https://breachreport.com/portal/user-api). Must be included in the request header. |
| email | string | Email address to be checked. |

</details>

### Code Examples

<details>
<summary>Shell command example</summary>
<br>

```shell
curl --location --request POST '{{BASE_URL}}/api/v1/email/check' \
--header 'api-key: {{API_KEY}}' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'email=test@test.com'
```
</details>

<details>
<summary>JavaScript code example</summary>
<br>

```javascript
//Using fetch()
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

fetch("{{BASE_URL}}/api/enterprise/v1/email/check", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

</details>

<details>
<summary>Python code example</summary>
<br>

```python
# Using requests
import requests
url = "{{BASE_URL}}/api/v1/email/check"
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
<summary>Ruby code example</summary>
<br>

```ruby
# Sample Ruby code
require "uri"
require "net/http"
url = URI("{{BASE_URL}}/api/v1/email/check")
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
<summary>Verified email address: found  breaches</summary>
<br>

```json
{
  "email": "john.smith@example.com",
  "records": 36,
  "isAssigned": false,
  "breaches": [
    {
      "breachId": 6800,
      "title": "BigDB Breach",
      "createdAt": "2019-03-06T14:22:21.579Z",
      "compromisedAccounts": 2680349475,
      "breachYear": 2019,
      "breachMonth": 0,
      "breachDay": 7,
      "url": "",
      "logo": "https://crm.breachreport.com/images/uploads/O4ts7dil8JYI_Joq.svg",
      "description": "On January 7th 2019 a 595 GB packed database named \"BigDB\" was leaked on the internet. Some emails can be found multiple times, as the dump aggregated 252 previous breaches, such as Anti Public and Exploit.in, and decrypted passwords of the known sites such as LinkedIn, Bitcoin and Pastebin.",
      "breachDataTypes": [
        "password",
        "email",
        "plaintext password"
      ]
    }
  ]
}

```

| Name | Type | Description |
| ------ | ------ | ------ |
| email | string | The email address. |
| records | integer | Incident count for the email address. |
| isAssigned | boolean | Indicator of whether the email is address is assigned to a domain: `true` or `false`. |
| breaches | list | List / array of data breach incidents and their description. |
| breachId | integer | The ID of the incident in the Breach Report Database. |
| title | string | The title of the incident in the Breach Report Database. |
| createdAt| string | The date the incident was added to the Breach Report Database. |
| compromisedAccounts | integer | The total number of compromised accounts. |
| breachYear | integer | Year the incident occured. |
| breachMonth | integer | Month of the incident. |
| breachDay | integer | Day of the incident. |
| url | string | The URL to the sources of the incident. |
| logo | string | The logo of the incident. |
| description | string | Text description of the incident. |
| breachDataTypes| list | List / array of exposed credential types in the incident. |

</details>


<details>
<summary>Unverified email address: No breaches</summary>
<br>

```json
{
    "email": "test@example.com",
    "records": 0,
    "isAssigned": false,
    "breaches": 0
}
```

| Name | Type | Description |
| ------ | ------ | ------ |
| email | string | Email that is checked. |
| records | integer | Email incidents count. The value will be 0, if no breach data in the database.  |
| isAssigned | boolean | Email verified by a user: true/false. |
| breaches | integer | Incident count for the email address. |

</details>


<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>

# Check a Hashed Email Address

**Request URL**: `{BASE_URL}/api/v1/email-hash/check`

**Request method:** `POST`

This API call accepts a SHA256-hash email address value. This is the recommended way to check email addresses using Breach Report API.

Alternatively, the API provides a request accepting [a plaintext email address value](#check-a-plaintext-email-address).

This API call returns:

* Breach count for **unverified** emails.
* Breach count and further details for **verified** emails.

How to construct the request:

1. Calculate your email address hash using **SHA256**.
2. Include the API key in the request header.
3. Specify your hashed email address value in the request body.


### About Email Address Hashing

Breach Report API only uses encrypted email address values. The encryption method is **Argond2d(SHA256)**.

Before sending a query, generate the email address hash.

<details>
<summary>How to produce an email address hash on Linux, on Mac OS or by using Git Bash on Windows.</summary>
<br>

1. Convert the email address to lowercase.
2. In the terminal, run the following command: `echo -n {email} | sha256sum`. Here, `{email}` is an email address you want to check. Don't use the brackets!
3. The command will produce a unique 64-character-long alphanumeric value that will look like: `8b063d4d3f323127ad8c13example69a747da2421db686144c5c982cc491e1ad`.

</details>

Alternatively, you may use an online hashing tool, for example, this [hash calculator on github.io](https://emn178.github.io/online-tools/sha256.html).

### Request Parameters

<details>
<summary>Show the parameters.</summary>
<br>

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | The key you can generate on the [portal](https://breachreport.com/portal/user-api). Must be included in the request header. |
| hash | string | Hashed email address you want to check. |

</details>

### Code Examples

<details>
<summary>Shell command example</summary>
<br>

```shell
curl --location --request POST '{{BASE_URL}}/api/v1/email-hash/check' \
--header 'api-key: {{API_KEY}}' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'hash=f660ab912ec121d1b1e928a0bb4bc61b15f5ad44d5efdc4e1c92a25e99b8e44a'
```
</details>

<details>
<summary>JavaScript code example</summary>
<br>

```javascript
// Using fetch()
var myHeaders = new Headers();
myHeaders.append("api-key", "{{API_KEY}}");
myHeaders.append("Content-Type", "application/x-www-form-urlencoded");
var urlencoded = new URLSearchParams();
urlencoded.append("hash", "f660ab912ec121d1b1e928a0bb4bc61b15f5ad44d5efdc4e1c92a25e99b8e44a");
var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: urlencoded,
  redirect: 'follow'
};
fetch("{{BASE_URL}}/api/enterprise/v1/email-hash/check", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

</details>

<details>
<summary>Python code example</summary>
<br>

```python
# Using requests
import requests
url = "{{BASE_URL}}/api/v1/email-hash/check"
payload = 'hash=f660ab912ec121d1b1e928a0bb4bc61b15f5ad44d5efdc4e1c92a25e99b8e44a'
headers = {
  'api-key': '{{API_KEY}}',
  'Content-Type': 'application/x-www-form-urlencoded'
}
response = requests.request("POST", url, headers=headers, data = payload)
print(response.text.encode('utf8'))
```

</details>

<details>
<summary>Ruby code example</summary>
<br>

```ruby
require "uri"
require "net/http"
url = URI("{{BASE_URL}}/api/v1/email-hash/check")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["api-key"] = "{{API_KEY}}"
request["Content-Type"] = "application/x-www-form-urlencoded"
request.body = "hash=f660ab912ec121d1b1e928a0bb4bc61b15f5ad44d5efdc4e1c92a25e99b8e44a"
response = http.request(request)
puts response.read_body
```

</details>

### Response examples

<details>
<summary>Verified Email Address - Found Breaches</summary>
<br>

```json
{
    "emailHash": "ab912ec121d1b1e928a0bb4bc61b15f5ad44d5efdc4e1c92a25e",
    "records": 21,
    "isAssigned": true,
    "breaches": [
        {
            "breachId": 865,
            "title": "Collection #2 ",
            "createdAt": "2019-03-06T14:21:14.374Z",
            "compromisedAccounts": 866851442,
            "breachYear": 2019,
            "breachMonth": 0,
            "url": "",
            "logo": "https://crm.breachreport.com/images/uploads/gbWusP5Upun2DLam.svg",
            "description": "As a part of the January 7th 2019 BigDB database leak, Collection #2 exposed more than 800M unique emails and passwords. The BigDB database does not feature new incidents, it is an aggregate file old incidents with newly decrypted passwords which the infosec community couldn't crack before. The file consists of the site data of the most famous services such as quifax and eBay.",
            "breachDataTypes": [
                "email",
                "plaintext password"
            ]
        }
    ]
}
```
| Name | Type | Description |
| ------ | ------ | ------ |
| emailHash | string | Hashed email address. |
| records | integer | Email incidents count. |
| isAssigned | boolean | The indicator whether the email is assigned: `true` or `false`. |
| breaches | list | List / array of data breach description items. |
| breachId | integer | The ID of the incident within Breach Report Database. |
| title | string | The title of the incident. |
| createdAt| string | The date the incident was added to our BR database. |
| compromisedAccounts | integer | Total number of accounts (email addresses and such) that were compromised in the breach. |
| breachYear | integer | The year the incident occured. |
| breachMonth | integer | The month of the incident. |
| url | string | The URL to the sources of the incident. |
| logo | string | The logo of the incident. |
| description | string | Text description of the incident. |
| breachDataTypes| [string] | The list of leaked data types within the incident. |
</details>

<details>
<summary>Unverified Email Address - Found Breaches</summary>
<br>

```
{
  "emailHash": "d5b474f8a5135b224905b124e32ff50d2f31d95e1b1cdb5c21c36d7a7db58dce",
  "records": 23,
  "isAssigned": false,
  "breaches": 4
}
```

| Name | Type | Description |
| ------ | ------ | ------ |
| emailHash | string | Hashed email address. |
| records | integer | Incident count for the email address. The value will be 0 (zero), if no matches. |
| isAssigned | boolean | Indicator of whether the email address is assigned to a domain: `true` or `false`. |
| breaches | integer | Number of data breach cases fior the email address. |

</details>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>

# Check a Domain for Compromised Email Addresses

**Request URL**: `{BASE_URL}/api/v1/domain/check`

**Request method:** `POST`

This API call accepts a plaintext web domain value and returns a list of compromised email addresses on this domain.

The method is only available to verified domain owners. Refer to the [Verifying a Domain](https://github.com/vassilyla/breachreport-api/blob/vassily/docs/01-before-using-api.md#verifying-a-domain) section for further information.

How to construct the request:

1. Include the API key in the request header.
2. Specify the domain in the request body.

### Request Parameters

<details>
<summary>Show the parameters.</summary>
<br>

| Name | Type | Description |
| ------ | ------ | ------ |
| api-key | string | The key you can generate on the [portal](https://breachreport.com/portal/user-api). Must be included in the request header. |
| domain | string | Web domain to be checked. |

</details>

### Code Examples

<details>
<summary>Shell command example</summary>
<br>

```shell
curl --location --request POST '{{BASE_URL}}/api/v1/domain/check' \
--header 'api-key: {{API_KEY}}' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'domain=example.com'
```
</details>

<details>
<summary>JavaScript code example</summary>
<br>

```javascript
// using fetch()
var myHeaders = new Headers();
myHeaders.append("api-key", "{{API_KEY}}");
var formdata = new FormData();
formdata.append("domain", "qip.com ");
var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: formdata,
  redirect: 'follow'
};

fetch("{{BASE_URL}}/api/enterprise/v1/domain/emails", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

</details>

<details>
<summary>Python code example</summary>
<br>

```python
# Using requests
import requests
url = "{{BASE_URL}}/api/v1/dcmain/check"
payload = 'domain=example.com'
headers = {
  'api-key': '{{API_KEY}}',
  'Content-Type': 'application/x-www-form-urlencoded'
}
response = requests.request("POST", url, headers=headers, data = payload)
print(response.text.encode('utf8'))
```

</details>

<details>
<summary>Ruby code example</summary>
<br>

```ruby
# Sample Ruby code
require "uri"
require "net/http"
url = URI("{{BASE_URL}}/api/v1/domain/check")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["api-key"] = "{{API_KEY}}"
request["Content-Type"] = "application/x-www-form-urlencoded"
request.body = "domain=example.com"
response = http.request(request)
puts response.read_body
```

</details>

### Response Examples

<details>
<summary>Found Compromised Email Addresses at this Domain</summary>
<br>

```json
{
  "emails": [
    "john.smith@example.com",
    "ivan.wagner@example.com",
    "admin@example.com"
  ]
}
```

| Name | Type | Description |
| ------ | ------ | ------ |
| emails | list | List / array of compromised email addresses.  |

</details>

<details>
<summary>Found No Compromised Email Addresses at this Domain</summary>
<br>

```json
{
    "emails": []
}
```

| Name | Type | Description |
| ------ | ------ | ------ |
| emails | list | Empty list / array for compromised email addresses. |

</details>

<details>
<summary>Cannot show the details for an unverified domain.</summary>
<br>

```json
{
    "emails": []
}
```

| Name | Type | Description |
| ------ | ------ | ------ |
| emails | list | Empty list / array for compromised email addresses. |

</details>

<p align="center">
  <br>
  <img width="500" src="./img/chapter-separate.jpg" alt="">
</p>
