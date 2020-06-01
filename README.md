# IPv6 DDNSPod Free API
This service offers a REST API allowing to get a visitor IPv6-Only address and to query location information from any IPv6 address. It outputs JSON-encoded IP geolocation data.

## GetIP (Get IPv6 address in plain text format):
Returns the visitor IPv6 address in plain text, useful for shell scripts or to find the external Internet routable address.

- Example (Plain text):  
 `https://ipv6.ddnspod.com`  
- Example (curl):  
 `curl ipv6.ddnspod.com`  

#### Usage example (Shell script): 

```shell
#!/bin/sh

ipv6=$(curl -s https://ipv6.ddnspod.com)
echo "My public IPv6 address is: $ipv6"
```  

## Splicing IPv6 (Get appending IPv6 address in plain text format):
Calling the API endpoint without any parameter will return IPv6 prefix for the visitor:

- Example (JSON):  
 `GET /prefix`  

Get IPv6 prefix for the visitor and Appending an IPv6 suffix as parameter return this IPv6 address:

- Example (JSON):  
 `GET /prefix/1:2:3:4`  
  or:  
 `GET /prefix/:5`  


## GeoIP (Get IPv6 address location in JSON format):
Calling the API endpoint without any parameter will return location information for the visitor IPv6 address:

- Example (JSON):  
 `GET /geoip`  
- Example (Format-JSON):  
 `GET /geoip/format`  


Appending an IP address as parameter will return location information for this IPv6 address:

- Example (JSON):  
 `GET /geoip/2400:3200::1`  

- Example (Format-JSON):  
 `GET /geoip/format/2400:3200::1`  


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
    "location": "中国浙江省杭州市",
    "isp": "阿里云计算有限公司"
  },
  "disp": "中国浙江省杭州市 阿里云计算有限公司",
  "timestamp": "1577844602020"
}
```  

## IPv6 Range to CIDR
Use CIDR notation to provide information about a given IPv6 address range.

- Enter the starting and ending IPv6 address respectively:  
 `GET /cidr/2400:3200::/2400:3200:ffff::`  

- Enter the starting and ending IPv6 address respectively (Format-JSON):  
 `GET /cidr/format/2400:3200::/2400:3200:ffff::`  

Output example:

```json
{
  "start": "2400:3200::",
  "end": "2400:3200:ffff::",
  "cidr": "2400:3200::/32"
}
```  

## Get Timestamp
Appending parameters to obtain timestamps with different precision.

- The second timestamps:  
 `GET /timestamp/s`  
 
- The millisecond timestamps (Default):  
 `GET /timestamp`  
  or:  
 `GET /timestamp/ms`  
 
- The microsecond timestamps:  
 `GET /timestamp/us`  
 
- The nanosecond timestamps:  
  `GET /timestamp/ns`  


## Errors
### Client Errors
> When incorrect user input is entered, the server returns an Error, along with a JSON-encoded error message.

