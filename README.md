# IPv6 DDNSPod Free API
This service offers a REST API allowing to get a visitor IPv4&IPv6 address and to query location information from any IPv6-Only address, It outputs JSON-encoded IP geolocation data.

## 1. GetIP (Get IPv4&IPv6 address in plain text format):
1.1 Returns the visitor IPv4&IPv6 address in plain text, useful for shell scripts or to find the external Internet routable address.

- Example (Plain text):  
```text
http(s)://ip.ddnspod.com
http(s)://ipv4.ddnspod.com
http(s)://ipv6.ddnspod.com
```
- Example (curl):  
```curl
curl ip.ddnspod.com
curl ip.ddnspod.com -4
curl ip.ddnspod.com -6
```

- Usage example (Shell script):  
```bash
#!/bin/bash
ipv4=$(curl -s https://ip.ddnspod.com -4)
ipv6=$(curl -s https://ip.ddnspod.com -6)
echo "My public IPv4 address is: $ipv4"
echo "My public IPv6 address is: $ipv6"
``` 
- Echo example:  
```bash
My public IPv4 address is: 1.2.3.4
My public IPv6 address is: 2001:2:3:4:5:6:7:8
``` 

## 2. Splicing IPv6 (Get appending IPv6 address in plain text format):
2.1 Calling the API endpoint without any parameter will return IPv6 prefix for the visitor:

- Example (Plain text):  
```text
http(s)://ip.ddnspod.com/prefix
```

2.2 Get IPv6 prefix for the visitor and Appending an IPv6 suffix as parameter return this IPv6 address:

- Example (Plain text):  
```text
GET /prefix/1:2:3:4
GET /prefix/:5
or
POST /prefix?suffix=1:2:3:4
POST /prefix?suffix=:5
```

## 3. GeoIP (Get IPv6 address location in JSON format):
3.1 Calling the API endpoint without any parameter will return location information for the visitor IPv6 address:

- Example (JSON):  
```text
http(s)://ip.ddnspod.com/geoip
```


3.2 Appending an IP address as parameter will return location information for this IPv6 address:

- Example (JSON):  
```text
GET /geoip/2400:3200::1
or
POST /geoip?ipv6=2400:3200::1
 ```


3.3 The output is a JSON object containing the following elements:

- Output example:

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
        "location": "中国 浙江省 杭州市",
        "isp": "阿里云计算有限公司"
    },
    "disp": "中国 浙江省 杭州市 阿里云",
    "timestamp": "943891200000"
}
```  

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


## 4. IPv6 Range to CIDR
4.1 Use CIDR notation to provide information about a given IPv6 address range.

- Enter the starting and ending IPv6 address respectively:  
```text
GET /cidr/2400:3200::/2400:3200:ffff::
or
POST /cidr?start=2400:3200::&end=2400:3200:ffff::
```

- Output example:

```json
{
    "start": "2400:3200::",
    "end": "2400:3200:ffff::",
    "cidr": "2400:3200::/32"
}
```  

## 5. Get Timestamp
5.1 The default is the millisecond timestamp:  

- Example (Plain text):  
```text
http(s)://ip.ddnspod.com/timestamp
```

- Appending parameters to obtain timestamps with different precision(second millisecond microsecond nanosecond).
```text
GET /timestamp/s
GET /timestamp/ms
GET /timestamp/us
GET /timestamp/ns
or
POST /timestamp?precision=s
POST /timestamp?precision=ms
POST /timestamp?precision=us
POST /timestamp?precision=ns
```


## Errors
### Client Errors
> When incorrect user input is entered, the server returns an Error, along with a JSON-encoded error message.
