
# DDNSPod Free API
This service offers a REST API allowing to get a visitor IPv6-Only address and to query location information from any IPv6 address. It outputs JSON-encoded IP geolocation data.

## GetIP (Get IPv6 address in plain text format):
Returns the visitor IPv6 address in plain text, useful for shell scripts or to find the external Internet routable address.

- Example (Plain text):  
 `https://ipv6.ddnspod.com`  
- Example (curl):  
 `curl ipv6.ddnspod.com`  

#### Usage example

C# :  
```C#
var httpClient = new HttpClient();
var ipv6 = await httpClient.GetStringAsync("https://ipv6.ddnspod.com");
Console.WriteLine($"My public IPv6 address is: {ipv6}");
```  

Go :  
```go
package main

import (
        "io/ioutil"
        "net/http"
        "os"
)

func main() {
        res, _ := http.Get("https://ipv6.ddnspod.com")
        ipv6, _ := ioutil.ReadAll(res.Body)
        os.Stdout.Write(ipv6)
}
```  

Java :  
```java
try (java.util.Scanner s = new java.util.Scanner(new java.net.URL("https://ipv6.ddnspod.com").openStream(), "UTF-8").useDelimiter("\\A")) {
    System.out.println("My public IPv6 address is: " + s.next());
} catch (java.io.IOException e) {
    e.printStackTrace();
}
```  

NodeJS :  
```node
var https = require("https");

https.get({"host": "ipv6.ddnspod.com", "port": 443, "path": "/"}, function(resp) {
  resp.on("data", function(ipv6) {
    console.log("My public IPv6 address is: " + ipv6);
  });
});
```  

Perl :  
```perl
use strict;
use warnings;
use LWP::UserAgent;

my $ua = new LWP::UserAgent();
my $ipv6 = $ua->get("https://ipv6.ddnspod.com")->content;
print "My public IPv6 address is: ". $ipv6;
```  

PHP :  
```php
$ipv6 = file_get_contents("https://ipv6.ddnspod.com");
echo "My public IPv6 address is: " . $ipv6;
```  

Python :  
```python
# This example requires the requests library be installed. You can learn more
# about the Requests library here: http://docs.python-requests.org/en/latest/
from requests import get

ipv6 = get("https://ipv6.ddnspod.com").text
print("My public IPv6 address is: {}".format(ipv6))
```  

Ruby :  
```ruby
require "net/http"

ipv6 = Net::HTTP.get(URI("https://ipv6.ddnspod.com"))
puts "My public IPv6 address is: " + ipv6
```   

Shell :  
```shell
#!/bin/sh

ipv6=$(curl -s https://ipv6.ddnspod.com)
echo "My public IPv6 address is: $ipv6"
```  


Output example:

```
2400:3200::1
```  

## Splicing IPv6 (Get appending IPv6 address in plain text format):
Calling the API endpoint without any parameter will return IPv6 prefix for the visitor:

- Example (JSON):  
 `https://ipv6.ddnspod.com/prefix`  

Get IPv6 prefix for the visitor and Appending an IPv6 suffix as parameter return this IPv6 address:

- Example (JSON):  
 `https://ipv6.ddnspod.com/prefix/1:2:3:4`  
  or:  
 `https://ipv6.ddnspod.com/prefix?suffix=1:2:3:4`  
- Example (JSON):  
 `https://ipv6.ddnspod.com/prefix/:5`  
  or:  
 `https://ipv6.ddnspod.com/prefix?suffix=:5`  

## GeoIP (Get IPv6 address location in JSON format):
Calling the API endpoint without any parameter will return location information for the visitor IPv6 address:

- Example (JSON):  
 `https://ipv6.ddnspod.com/geoip`  
- Example (Format-JSON):  
 `https://ipv6.ddnspod.com/geoip/format`  
  or:  
 `https://ipv6.ddnspod.com/geoip?format=yes`  

Appending an IP address as parameter will return location information for this IPv6 address:

- Example (JSON):  
 `https://ipv6.ddnspod.com/geoip/2400:3200::1`  
  or:  
 `https://ipv6.ddnspod.com/geoip?ipv6=2400:3200::1`  
- Example (Format-JSON):  
 `https://ipv6.ddnspod.com/geoip/format/2400:3200::1`  
  or:  
 `https://ipv6.ddnspod.com/geoip/format?ipv6=2400:3200::1`  
  or:  
 `https://ipv6.ddnspod.com/geoip?format=yes&ipv6=2400:3200::1`  

#### GeoIP Output Schema
The output is a JSON object containing the following elements:

|      Parameter       |                        Description                            |
| -------------------- | ------------------------------------------------------------- |
| **`ipv6`**           | Visitor IPv6 address, or IPv6 address specified as parameter. |
| **`ipv6-full`**      | IPv6 full 128-bit notation.                                   |
| **`range->start`**   | The start IPv6 prefix for an IPv6 address range.              |
| **`range->end`**     | The end IPv6 prefix for an IPv6 address range.                |
| **`cidr`**           | The CIDR notation for an IPv6 addresses range.                |
| **`addr->location`** | Name of the country,province,city,region.                     |
| **`addr->isp`**      | ISP name.                                                     |
| **`disp`**           | Location + ISP name.                                          |

Output example:

```json
{
    "ipv6": "2400:3200::1",
    "ipv6-full": "2400:3200:0000:0000:0000:0000:0000:0001",
    "range": {
        "start": "2400:3200::",
        "end": "2400:3200:15ff:ffff::"
    },
    "cidr": "2400:3200::/35",
    "addr": {
        "location": "中国浙江省杭州市",
        "isp": "阿里云计算有限公司"
    },
    "disp": "中国浙江省杭州市 阿里云计算有限公司"
}
```  

## IPv6 Range to CIDR
Use CIDR notation to provide information about a given IPv6 address range.

- Enter the starting and ending IPv6 address respectively:  
 `https://ipv6.ddnspod.com/cidr/2400:3200::/2400:3200:ffff::`  
  or:  
 `https://ipv6.ddnspod.com/cidr?start=2400:3200::&end=2400:3200:ffff::`  
- Enter the starting and ending IPv6 address respectively (Format-JSON):  
 `https://ipv6.ddnspod.com/cidr/format/2400:3200::/2400:3200:ffff::`  
  or:  
 `https://ipv6.ddnspod.com/cidr?format=yes&start=2400:3200::&end=2400:3200:ffff::`  

Output example:

```json
{
    "start": "2400:3200::",
    "end": "2400:3200:ffff::",
    "cidr": "2400:3200::/32"
}
```  

## Errors
### Client Errors
> When incorrect user input is entered, the server returns an Error, along with a JSON-encoded error message.


