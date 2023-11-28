# akeneo-openapi
OpenAPI spec for the Akeneo PIM REST API

## Python example

First, follow the instructions to install [Swagger Codegen](https://github.com/swagger-api/swagger-codegen) and generate a python client.

The following is a code snippet to authenticate and fetch a product using a UUID.

```python
from __future__ import print_function
import base64
import json
import swagger_client
from swagger_client.rest import ApiException
from swagger_client.configuration import Configuration

# START CONFIG ----------------------------------------------------------

host = ""
username = ""
password = ""
client_id = ""
secret = ""

# END CONFIG -------------------------------------------------------------

configuration = Configuration()
configuration.host = host
configuration.username = username
configuration.password = password
configuration.api_key_prefix = {"Authorization": "Bearer"}

auth = "Basic " + base64.b64encode("{}:{}".format(client_id, secret).encode()).decode('ascii')

authBody = {
    "grant_type": "password",
    "username": configuration.username,
    "password": configuration.password
}

auth_api = swagger_client.AuthenticationApi(swagger_client.ApiClient(configuration))

try:
    auth_response = auth_api.post_token(content_type='', authorization=auth, body=authBody)
 
    configuration.api_key = {"Authorization": auth_response.access_token}

    api_instance = swagger_client.ProductUuidApi(swagger_client.ApiClient(configuration))

    response = api_instance.get_products_uuid_uuid("your-product-uuid", _preload_content=False)

    product = json.loads(response.data)

    print(product)
except ApiException as e:
    print("Exception when calling ProductUUidApi->get: %s\n" % e)
```
