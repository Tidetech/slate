# /v2/datasets/{group}/{dataset}/area/

**Summary:** Raster data from a dataset

**Description:** The /v2/datasets/{group}/{dataset}/area/ endpoint extracts
data from a dataset over an entire area. The endpoint only
accepts POST requests. POST requests must be in JSON format, 
with the appropriate Content-Type: application/json headers set.

**Endpoint** `/v2/datasets/{group}/{dataset}/area/`

**Method** `POST`


``` shell
curl --location --request POST https://api.tidetech.org/v2/datasets/meteorology/global_meteorology/area/ \
--user 'my_api_key:my_api_secret' \
--header 'Content-Type: application/json' \
--data-raw '{
    "start_timestep": "2020-05-27T00:00:00Z",
    "end_timestep": "2020-05-27T12:00:00Z",
    "parameters": ["UGRD", "VGRD"],
    "polygon": {
        "type": "Polygon",
        "coordinates": [[
            [-1.2732, 50.8536],
            [-1.7080, 50.6650],
            [-1.2732, 50.5020],
            [-0.8165, 50.6691],
            [-1.2732, 50.8536]
        ]]
    }
}' \
--output global_met.nc
```

``` javascript
const fs = require('fs')
const axios = require('axios')
const apikey = 'my_api_key'
const apisecret = 'my_api_secret'
const data = {"start_timestep":"2020-05-27T00:00:00Z","end_timestep":"2020-05-27T12:00:00Z","parameters":["UGRD","VGRD"],"polygon":{"type":"Polygon","coordinates":[[[-1.2732,50.8536],[-1.708,50.665],[-1.2732,50.502],[-0.8165,50.6691],[-1.2732,50.8536]]]}}

axios.post("https://api.tidetech.org/v2/datasets/meteorology/global_meteorology/area/", data, {
    responseType: 'stream',
    auth: {
        username: apikey,
        password: apisecret,
    }
}).then((response) => {
    response.data.pipe(fs.createWriteStream("global_met.nc"))
}).catch((error) => {
    console.log(error);
})
```

``` python
from requests import request

url = "https://api.tidetech.org/v2/datasets/meteorology/global_meteorology/area/"

payload = {"start_timestep":"2020-05-27T00:00:00Z","end_timestep":"2020-05-27T12:00:00Z","parameters":["UGRD","VGRD"],"polygon":{"type":"Polygon","coordinates":[[[-1.2732,50.8536],[-1.708,50.665],[-1.2732,50.502],[-0.8165,50.6691],[-1.2732,50.8536]]]}}

r = request("POST", url, json=payload, auth=("my_api_key", "my_api_secret"), stream=True)

with open('global_met.nc', 'wb') as f:
    for chunk in r.iter_content(chunk_size=1024*1024):
        f.write(chunk)
```

``` csharp
var apikey = "my_api_key";
var apisecret = "my_api_secret";

var client = new RestClient("https://api.tidetech.org/v2/datasets/meteorology/global_meteorology/area/");
client.Authenticator = new HttpBasicAuthenticator(apikey, apisecret);
client.Timeout = -1;

var request = new RestRequest(Method.POST);
request.AddHeader("Content-Type", "application/json");
request.AddParameter(
    "application/json",
    "{\"start_timestep\":\"2020-05-27T00:00:00Z\",\"end_timestep\":\"2020-05-27T12:00:00Z\",\"parameters\":[\"UGRD\",\"VGRD\"],\"polygon\":{\"type\":\"Polygon\",\"coordinates\":[[[-1.2732,50.8536],[-1.708,50.665],[-1.2732,50.502],[-0.8165,50.6691],[-1.2732,50.8536]]]}}",
    ParameterType.RequestBody);

var response = client.DownloadData(request);
File.WriteAllBytes("global_met.nc", response);
```

``` go
package main

import (
    "fmt"
    "strings"
    "net/http"
    "io/ioutil"
)

const (
    apikey = "my_api_key"
    apisecret = "my_api_secret"
)

func main() {

    url := "https://api.tidetech.org/v2/datasets/meteorology/global_meteorology/area/"
    method := "POST"

    payload := strings.NewReader("{\"start_timestep\":\"2020-05-27T00:00:00Z\",\"end_timestep\":\"2020-05-27T12:00:00Z\",\"parameters\":[\"UGRD\",\"VGRD\"],\"polygon\":{\"type\":\"Polygon\",\"coordinates\":[[[-1.2732,50.8536],[-1.708,50.665],[-1.2732,50.502],[-0.8165,50.6691],[-1.2732,50.8536]]]}}")

    client := &http.Client {}
    req, err := http.NewRequest(method, url, payload)

    if err != nil {
        fmt.Println(err)
    }
    req.SetBasicAuth(apikey, apisecret)
    req.Header.Add("Content-Type", "application/json")
    res, err := client.Do(req)
    defer res.Body.Close()
    data, _ := ioutil.ReadAll(res.Body)

    ioutil.WriteFile("global_met.nc", data, 0777)
}
```



**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| group | path | dataset group id | Yes | string |
| dataset | path | dataset id | Yes | string |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Returns raster data in GRIB or NetCDF format |

> The above command returns area data in GRIB or NetCDF format