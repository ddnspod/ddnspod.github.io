# IPv6 DDNSPod Free API
This service offers a REST API allowing to get a visitor IPv4&IPv6 address and to query location information from any Only-IPv6 address. It outputs JSON-encoded IP geolocation data.

## GetIP (Get IPv4&IPv6 address in plain text format):
Returns the visitor IPv4&IPv6 address in plain text, useful for shell scripts or to find the external Internet routable address.

- Example (Plain text):  
```text
https://ip.ddnspod.com
https://ipv4.ddnspod.com
https://ipv6.ddnspod.com
```
- Example (curl):  
```curl
curl ip.ddnspod.com
curl ip.ddnspod.com -4
curl ip.ddnspod.com -6
```

#### Usage example (Shell script): 
```bash
#!/bin/bash

ipv4=$(curl -s https://ip.ddnspod.com -4)
ipv6=$(curl -s https://ip.ddnspod.com -6)
echo "My public IPv4 address is: $ipv4"
echo "My public IPv6 address is: $ipv6"
```  

#### Echo example: 
```bash
My public IPv4 address is: 1.2.3.4
My public IPv6 address is: 2001:2:3:4:5:6:7:8
``` 

## Splicing IPv6 (Get appending IPv6 address in plain text format):
Calling the API endpoint without any parameter will return IPv6 prefix for the visitor:

- Example (Plain text):  
```text
GET /prefix
```

Get IPv6 prefix for the visitor and Appending an IPv6 suffix as parameter return this IPv6 address:

- Example (Plain text):  
```text
GET /prefix/1:2:3:4
GET /prefix/:5
or
GET /prefix?suffix=1:2:3:4
GET /prefix?suffix=:5
```

## GeoIP (Get IPv6 address location in JSON format):
Calling the API endpoint without any parameter will return location information for the visitor IPv6 address:

- Example (JSON):  
```text
GET /geoip
GET /geoip/format
```


Appending an IP address as parameter will return location information for this IPv6 address:

- Example (JSON):  
```text
GET /geoip/2400:3200::1
GET /geoip/format/2400:3200::1
or
GET /geoip?ipv6=2400:3200::1
GET /geoip/format?ipv6=2400:3200::1
 ```


#### GeoIP Output Schema
The output is a JSON object containing the following elements:

|      Parameter       |                        Description                            |
| -------------------- | ------------------------------------------------------------- |
| **ipv6**             | Visitor IPv6 address, or IPv6 address specified as parameter. |
| **full**             | IPv6 full 128-bit notation.                                   |
| **range->start**     | The start IPv6 prefix for an IPv6 address range.              |
| **range->end**       | The end IPv6 prefix for an IPv6 address range.                |
| **cidr**             | The CIDR notation for an IPv6 addresses range.                |
| **addr->location**   | Name of the country,province,city,region.                     |
| **addr->isp**        | ISP name.                                                     |
| **disp**             | Location + ISP name.                                          |
| **timestamp**        | The current timestamp.                                        |

Output example:

```json
{
  "ipv6": "2400:3200::1",
  "full": "2400:3200:0000:0000:0000:0000:0000:0001",
  "range": {
    "start": "2400:3200::",
    "end": "2400:3200:15ff:ffff::"
  },
  "cidr": "2400:3200::/35",
  "addr": {
    "location": "????????????????????????",
    "isp": "???????????????????????????"
  },
  "disp": "???????????????????????? ???????????????????????????",
  "timestamp": "1577844602020"
}
```  

## IPv6 Range to CIDR
Use CIDR notation to provide information about a given IPv6 address range.

- Enter the starting and ending IPv6 address respectively:  
```text
GET /cidr/2400:3200::/2400:3200:ffff::
GET /cidr/format/2400:3200::/2400:3200:ffff::
or
GET /cidr?start=2400:3200::&end=2400:3200:ffff::
GET /cidr/format?start=2400:3200::&end=2400:3200:ffff::
```

Output example:

```json
{
  "start": "2400:3200::",
  "end": "2400:3200:ffff::",
  "cidr": "2400:3200::/32"
}
```  

## Get Timestamp
Appending parameters to obtain timestamps with different precision(second millisecond microsecond nanosecond).
```text
GET /timestamp/s
GET /timestamp/ms
GET /timestamp/us
GET /timestamp/ns
or
GET /timestamp?precision=s
GET /timestamp?precision=ms
GET /timestamp?precision=us
GET /timestamp?precision=ns
```

- The default is the millisecond timestamp:  
```text
GET /timestamp
```

## Errors
### Client Errors
> When incorrect user input is entered, the server returns an Error, along with a JSON-encoded error message.

