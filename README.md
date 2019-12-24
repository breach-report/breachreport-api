# Introduction

BreachReport allows to use our publicly available API calls to check emails and domains for breaches. API is accessible via Secure HTTP. 
Visit BreachReport [website](https://breachreport.com) for all features.

**Accessing the API**
The BreachReport API (v1) may be accessed by sending Secure HTTP GET/POST requests to URLs in the following format:

`https://breachreport.com/api/v1/<resource>/<function>` and passing the request payload 

For example, to check how many breaches we have for the email address test@test.com, you would send the following request:

`GET https://breachreport.com/api/v1/email/check -d "email=test@test.com"`

	
**API interfaces:**
* Check an email for breaches.
* Check a domain for breaches.

> You will need to register on [https://breachreport.com](https://breachreport.com) prior to using our API. 

# Authorization
All API calls require to add an API Key to the header of each request. Getting a key can be done in three easy steps:
1. [Register](https://breachreport.com/signup) or [login](htpps://breachreport.com/login) at our website.
2. Generate an API Key by clicking on `Create key` in **API** section.
3. Include the **API-Key** in every API call as `Header`.

> Note that you may need to upgrade your [Subscription](https://breachreport.com/portal/subscriptions) to get more API calls.

# Email check
> To avoid passing plain email address over the network, we do recommend to use hashed email address search described [below](https://github.com/breach-report/breachreport-api/wiki#hashed-email-check)

**Email check** returns:
* Breaches count for **unverified** emails.
* Breaches count and details for **verified** emails.

The email check can be done couple of steps:
1. Hash your email with **SHA-256**.
2. Include API Key as an `API-Key` header.
3. Include your hashed email in body request.

## Request example
**Request URL**: `https://breachreport.com/api/v1/email/check`
**Request method**: `POST`


```shell
curl --location --request POST 'https://breachreport.com/portal/api/v1/email/check' \
--header 'API-Key: 916f1847-6a59-4e1b-67fb-71c12265cade' \
--form 'email=goodfig1@yahoo.com'
```
**Request parameters:**
| Name | Type | Description |
| ------ | ------ | ------ |
| API-Key | string | An API Key retrieved from [Portal](https://breachreport.com/portal/user-api). Should be included as `Header`. |
| email | string | Email you want to check. |

#### Verified email response parameters:
| Name | Type | Description |
| ------ | ------ | ------ |
| emailHash | string | SHA-256 hash of your email. |
| records | integer | Breaches count. |
| isAssigned | boolean | Email verified by a user: true/false. |
| breaches | null / {} | Detailed breach information. If `isAssigned = true`, `breaches` will include extensive information. |
| breachId | integer | Breach ID at BreachReport. |
| title | string | Title of the Breach. |
| password | string | Leaked password. In case, if the password is hashed, the `password` will contain hash. |
| passwordAlgorithm | string | Hashing type of the password. If there was no hashing to password, the value will be `plaintext`. |
| passwordSalt | string | Password salt. If there is no salt, the value will be empty. |

#### Unverified email reponse parameters:
| Name | Type | Description |
| ------ | ------ | ------ |
| emailHash | string | SHA-256 hash of your email. |
| records | integer | Breaches count. |
| isAssigned | boolean | Email verified by a user: true/false. |
| breaches | null / {} | Detailed breach information. If `isAssigned = false`, then breaches will be null. |

#### Verified email response example

```
{
  "email": "goodfig1@yahoo.com",
  "records": 14,
  "isAssigned": true,
  "breaches": [
    {
      "breachId": 6800,
      "title": "BigDB Combolist",
      "password": "thatispassword",
      "passwordAlgorithm": "plaintext",
      "passwordSalt": ""
    }
  ]
}
```

#### Unverified email example

```
{“emailHash":"8b063d4d3f323127ad8c13d42207269a747da2421db686144c5c982cc491e1da","records":27,"isAssigned":false,"breaches":null} 
```


# Hashing
We **do** care about security, and take extensive safety measures: information about emails stored in our DB is hashed with set of hashes **Argond2d(SHA256(<email>))**. 
Before sending queries, make sure that you hashed your given email address with **SHA256**.

## Linux (Mac OS)
Run next commands in your shell terminal:

Hash email address with SHA256:
```
echo -n {your email} | sha256sum
```

Where `{your email}` is an email that you want to check. Note, that brackets should not be used.

The SHA256 hash looks like: `8b063d4d3f323127ad8c13d42207269a747da2421db686144c5c982cc491e1ad`

## Online hashing tool
You can also use online tools for hashing. For example, [github.io](https://emn178.github.io/online-tools/sha256.html) source allows a wide variety for hashing.

# Hashed email check

**Hashed email check** returns:
* Breaches count for **unverified** emails.
* Breaches count and details for **verified** emails.

The email check can be done couple of steps:
1. Hash your email with **SHA256**.
2. Include API Key as an `API-Key` header.
3. Include your hashed email in body request.

## Request examples
**Request URL**: `https://breachreport.com/api/v1/email-hash/check`
**Request method**: `POST`

**Request parameters:**
| Name | Type | Description |
| ------ | ------ | ------ |
| API-Key | string | An API Key retrieved from [Portal](https://breachreport.com/portal/user-api). Should be included as `Header`. |
| hash | string | SHA-256 hash of your email. |

**Unverified email reposnse parameters:**
| Name | Type | Description |
| ------ | ------ | ------ |
| emailHash | string | SHA-256 hash of your email. |
| records | integer | Breaches count. |
| isAssigned | boolean | Email verified by a user: true/false. |
| breaches | null / {} | Detailed breach information. If `isAssigned = false`, then breaches will be null. |


```shell
curl https://breachreport.com/api/v1/email-hash/check -d "hash=8b063d4d3f323127ad8c13d42207269a747da2421db686144c5c982cc491e1da" -H "API-Key: 76dbd8a7-7614-49db-ba08-93926fe404df” 
```

```python
import requests

headers = {
    'API-Key': '76dbd8a7-7614-49db-ba08-93926fe404df\u201D',
}

data = {
  'hash': '8b063d4d3f323127ad8c13d42207269a747da2421db686144c5c982cc491e1da'
}

response = requests.post('https://breachreport.com/api/v1/email-hash/check', headers=headers, data=data)
```

```php

$ch = curl_init();

curl_setopt($ch, CURLOPT_URL, 'https://breachreport.com/api/v1/email-hash/check');
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, "hash=8b063d4d3f323127ad8c13d42207269a747da2421db686144c5c982cc491e1da");

$headers = array();
$headers[] = 'API-Key: 76dbd8a7-7614-49db-ba08-93926fe404df”';
$headers[] = 'Content-Type: application/x-www-form-urlencoded';
curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);

$result = curl_exec($ch);
if (curl_errno($ch)) {
    echo 'Error:' . curl_error($ch);
}
curl_close($ch);
```

```node.js
var request = require('request');

var headers = {
    'API-Key': '76dbd8a7-7614-49db-ba08-93926fe404df”'
};

var dataString = 'hash=8b063d4d3f323127ad8c13d42207269a747da2421db686144c5c982cc491e1da';

var options = {
    url: 'https://breachreport.com/api/v1/email-hash/check',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

```c#
using (var httpClient = new HttpClient())
{
    using (var request = new HttpRequestMessage(new HttpMethod("POST"), "https://breachreport.com/api/v1/email-hash/check"))
    {
        request.Content = new StringContent("hash=8b063d4d3f323127ad8c13d42207269a747da2421db686144c5c982cc491e1da");
        request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/x-www-form-urlencoded"); 

        var response = await httpClient.SendAsync(request);
    }
}
```

```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
)

func main() {
	client := &http.Client{}
	var data = []byte(`{hash=8b063d4d3f323127ad8c13d42207269a747da2421db686144c5c982cc491e1da}`)
	req, err := http.NewRequest("POST", "https://breachreport.com/api/v1/email-hash/check", data)
	if err != nil {
		log.Fatal(err)
	}
	req.Header.Set("API-Key", "76dbd8a7-7614-49db-ba08-93926fe404df”")
	resp, err := client.Do(req)
	if err != nil {
		log.Fatal(err)
	}
	bodyText, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("%s\n", bodyText)
}
```


## Response example
> Unverified email example

```
{“emailHash":"8b063d4d3f323127ad8c13d42207269a747da2421db686144c5c982cc491e1da","records":27,"isAssigned":false,"breaches":null} 
```


> Verified email example
```
{
  "email": "goodfig1@yahoo.com",
  "records": 14,
  "isAssigned": true,
  "breaches": [
    {
      "breachId": 6800,
      "title": "BigDB Combolist",
      "password": "thatispassword",
      "passwordAlgorithm": "plaintext",
      "passwordSalt": ""
    }
  ]
}
```
| Name | Type | Description |
| ------ | ------ | ------ |
| emailHash | string | SHA-256 hash of your email. |
| records | integer | Breaches count. |
| isAssigned | boolean | Email verified by a user: true/false. |
| breaches | null / {} | Detailed breach information. If `isAssigned = true`, `breaches` will include extensive information. |
| breachId | integer | Breach ID at BreachReport. |
| title | string | Title of the Breach. |
| password | string | Leaked password. In case, if the password is hashed, the `password` will contain hash. |
| passwordAlgorithm | string | Hashing type of the password. If there was no hashing to password, the value will be `plaintext`. |
| passwordSalt | string | Password salt. If there is no salt, the value will be empty. |

# Status codes
| Code | Name | Description |
| ------ | ------ | ------ |
| 200 | OK | Request successfully passed. |
| 400 | Bad Request | Invalid domain URL. Please check the Base URL. |
| 401 | Unauthorized | Missing `API-Key` header. Make sure that you have generated one at [Portal](https://breachreport.com/portal/user-api) section. |
| 402 | Payment required | You need to upgrade subscription. You can visit [Subscription](https://breachreport.com/portal/subscriptions) page. |
| 409 | Conflict | A domain name or email already is already registered. Check your account for existing domains/emails. |
| 500 | Internal server error | Internal server error occured. If this continues, please, contact our Support. |
