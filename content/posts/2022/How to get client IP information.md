---
title: "How to Get Client IP Information?"
date: 2022-07-29T09:21:08-07:00
categories:
  - Tech
  - Security
tags:
  - IP
  - APP Engine
  - Go
draft: false
---

### How to get client IP address?
The `X-Forwarded-For` (XFF) HTTP header is a de facto standard for identifying the originating IP address of a client 
connecting to a web server through an HTTP proxy or load balancer.

Example code for getting IP by using Go
```
var ErrInvalidIP error = errors.New("invalid IP address")

func GetIP(r *http.Request) (string, error) {
	// 1. X-REAL-IP: Nginx
	rawIP := r.Header.Get("X-REAL-IP")
	if ip := net.ParseIP(rawIP); ip != nil {
		return rawIP, nil
	}

	// 2. X-FORWARDED-FOR
	rawIPs := r.Header.Get("X-FORWARDED-FOR")
	// Get the first valid IP
	for _, rawIP := range strings.Split(rawIPs, ",") {
		if ip := net.ParseIP(rawIP); ip != nil {
			return rawIP, nil
		}
	}

	// 3. RemoteAddr
	rawIP, _, err := net.SplitHostPort(r.RemoteAddr)
	if err != nil {
		return "", ErrInvalidIP
	}
	if ip := net.ParseIP(rawIP); ip != nil {
		return rawIP, nil
	}

	return "", ErrInvalidIP
}
```

### Google App Engine and IP Information
If your website is hosted on Google App Engine, it'll be very easy to get these (accurate than most online services) information from the headers:
```
X-Appengine-City:[richmond] X-Appengine-Citylatlong:[49.166590,-123.133569] 
X-Appengine-Country:[CA] 
X-Appengine-Default-Version-Hostname:[myip-123.uw.r.appspot.com] 
X-Appengine-Https:[on] 
X-Appengine-Region:[bc] 
X-Appengine-User-Ip:[207.102.244.236] 
```

When using Google APP Engine, it's more reliable to get the IP information from `X-Appengine-*` header.
```
type IPInfo struct {
	IP          string
	Country string
	Region  string
	City    string
}

func GetAPPEngineIPInfo(r *http.Request) IPInfo {
	ip := r.Header.Get("X-Appengine-User-Ip")
	country := r.Header.Get("X-Appengine-Country")
	region := r.Header.Get("X-Appengine-Region")
	city := r.Header.Get("X-Appengine-City")
	return IPInfo{
		IP:          ip,
		Country: country,
		Region:  region,
		City:    city,
	}
}
```

Here is the complete `http.Request` information from a website in Google App Engine:
```
&{
GET / HTTP/1.1
 %!s(int=1) %!s(int=1) 
 map[Accept:[text/html,application/xhtml+xml,application/xml;
 q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9] 
 Accept-Encoding:[gzip, deflate, br] 
 Accept-Language:[en-CA,en;q=0.9,zh-CN;q=0.8,zh;q=0.7] 
 Cache-Control:[max-age=0] 
 Forwarded:[for="207.102.244.236";proto=https] 
 Sec-Ch-Ua:[".Not/A)Brand";v="99", "Google Chrome";v="103", "Chromium";v="103"] 
 Sec-Ch-Ua-Mobile:[?0] Sec-Ch-Ua-Platform:["Windows"] Sec-Fetch-Dest:[document] 
 Sec-Fetch-Mode:[navigate] Sec-Fetch-Site:[none] Sec-Fetch-User:[?1] 
 Traceparent:[00-ab8d973d746241e8c5fdaf3c5f93b0be-f88fd0098e5cdd2b-01] 
 Upgrade-Insecure-Requests:[1] 
 User-Agent:[Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36] 
 X-Appengine-Api-Ticket:[ChAyYzZkODdhNGZlYzM3NDE5EIS8rRcQk7ytFxDl3rEXEPTesRcQ1tLCFxDc0sIXENGmyRcQ4KbJFxoTCM3YjbDXnvkCFYXW/AodksYFew==] 
 X-Appengine-City:[richmond] 
 X-Appengine-Citylatlong:[49.166590,-123.133569] 
 X-Appengine-Country:[CA] 
 X-Appengine-Default-Version-Hostname:[myip-123.uw.r.appspot.com] 
 X-Appengine-Https:[on] 
 X-Appengine-Region:[bc] 
 X-Appengine-Request-Log-Id:[62e420d500ff094d0dec703b6d00017a75777e6d7969702d3132330001323032323037323974303831313538000100] 
 X-Appengine-Timeout-Ms:[599999] 
 X-Appengine-User-Ip:[207.102.244.236] 
 X-Cloud-Trace-Context:[ab8d973d746241e8c5fdaf3c5f93b0be/17910762982537485611;o=1] 
 X-Forwarded-For:[207.102.244.236, 169.254.1.1] 
 X-Forwarded-Proto:[https] 
 X-Google-Serverless-Node-Envoy-Config-Gae:[]] 
 {} %!s(func() (io.ReadCloser, error)=<nil>) %!s(int64=0) [] %!s(bool=false) 
 myip-123.uw.r.appspot.com 
 map[] map[] %!s(*multipart.Form=<nil>) map[] 
 127.0.0.1:44754 / %!s(*tls.ConnectionState=<nil>) 
 %!s(<-chan struct {}=<nil>) %!s(*http.Response=<nil>) 
 %!s(*context.cancelCtx=&{0xc000084300 {0 0} <nil> map[] <nil>})}
```

## Pitfalls 
### Why the city information is different from different online services for the same IP address?
The same IP address `69.158.251.18` have different city name:
`Richmond`(British Columbia): ip2location.io (Best)
`Burnaby`(British Columbia): ipwhois.io
`Vancouver`(British Columbia): ipinfo.io
`Sainte-Agathe-des-Monts`(Quebec): ip-api.com (Worst)

There are many factors as to why the geolocation data results are not 100% accurate. Factors include:
* Where the IP address was registered
* Where the controlling agency is located
* Whether the user has a proxy or VPN
* Whether the connection is cellular or not

For example, I connected to internet by using Internet Service Provider (ISP, e.g. Shaw or Telus in our city) router. 
Only the ISP knows exactly which computer IP you have, but the online IP Lookup service only sees the public IP address from you ISP.

The ISP has services for Vancouver, Burnaby and other cities. The same public IP may have users in different cities. 

From the [IP Geolocation Data Accuracy](https://www.ip2location.com/data-accuracy) report, 
the threshold of accuracy is less than 50 miles, the accuracy is around 80% in average.

### Why I get different IP addresses from different services for my computer?
I got two different IP addresses from these online services:
* `69.158.251.18`: ipinfo.io, ip2location.io, etc.
* `207.102.244.236`: whatismyipaddress.com, showmyip.com, etc.

It's caused by using different route to different services: 
```
G:\>tracert ipinfo.io

Tracing route to ipinfo.io [34.117.59.81]
over a maximum of 30 hops:

  1    <1 ms    <1 ms     1 ms  172.28.2.6
  2    <1 ms    <1 ms    <1 ms  10.92.0.33
  3    <1 ms    <1 ms    <1 ms  10.92.0.43
  4    <1 ms    <1 ms    <1 ms  192.168.254.5
  5     2 ms     2 ms     1 ms  69.158.251.17
  6     4 ms     4 ms     5 ms  172.21.173.233
  7     6 ms     8 ms    10 ms  core1vancouver_9-1-0.net.bell.ca [64.230.123.248]
  8     9 ms    10 ms    10 ms  tcore3-seattle_hundredgige0-5-0-0.net.bell.ca [64.230.79.92]
  9     7 ms     7 ms     9 ms  bx5-seattle_bundle-ether9.net.bell.ca [64.230.125.245]
 10     7 ms     7 ms     7 ms  google-peering.net.bell.ca [64.230.186.30]
 11     9 ms     8 ms     8 ms  142.251.70.99
 12     7 ms     8 ms     8 ms  209.85.254.171
 13     6 ms     6 ms     7 ms  81.59.117.34.bc.googleusercontent.com [34.117.59.81]

Trace complete.

G:\>tracert myip-123.uw.r.appspot.com

Tracing route to myip-123.uw.r.appspot.com [142.250.69.212]
over a maximum of 30 hops:

  1     1 ms    <1 ms    <1 ms  172.28.2.6
  2    <1 ms    <1 ms    <1 ms  10.92.0.33
  3    <1 ms    <1 ms    <1 ms  10.92.0.43
  4     1 ms     1 ms     1 ms  207.194.4.33
  5     8 ms     4 ms     1 ms  host11.220.108.206.in-addr.arpa [206.108.220.11]
  6     7 ms     7 ms     7 ms  154.11.12.219
  7     8 ms     7 ms     8 ms  74.125.50.110
  8     8 ms     8 ms     8 ms  74.125.243.177
  9    11 ms     7 ms     7 ms  142.251.48.211
 10     7 ms     7 ms     7 ms  sea30s08-in-f20.1e100.net [142.250.69.212]

Trace complete.

```

### The `X-Forwarded-For` and `X-REAL-IP` headers are untrustworthy
These headers can be modified by using curl or other software.
```
PS C:\> curl -X GET https://myip-123.uw.r.appspot.com/ -H "X-Forwarded-For: 1.2.3.4" -H "X-REAL-IP: 1.2.3.4" -H 'Forwarded: for="1.2.3.4"'
...
Forwarded:[for=1.2.3.4,for="207.102.244.236";proto=https] 
X-Forwarded-For:[1.2.3.4,207.102.244.236, 169.254.1.1] X-Forwarded-Proto:[https]
X-Real-Ip:[1.2.3.4]] {} 
...
```

### Client IP address can't identify a client
* Client IP addresses describe only the computer being used, not the user. If multiple users share the same computer, they will be indistinguishable.
* Many Internet service providers dynamically assign IP addresses to users when they log in. Each time they log in, they get a different address, so web servers can’t assume that IP addresses will identify a user across login sessions.
* To enhance security and manage scarce addresses, many users browse the Internet through Network Address Translation (NAT) firewalls. These NAT devices obscure the IP addresses of the real clients behind the firewall, converting the actual client IP address into a single, shared firewall IP address (and different port numbers).
* HTTP proxies, etc

## Useful Links
* [MDN - X-Forwarded-For](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Forwarded-For)
* [Google APP Engine Request Headers and Responses](https://cloud.google.com/appengine/docs/standard/go/reference/request-response-headers)
* [Real Remote Client IP Addresses](https://docs.fastly.com/signalsciences/faq/real-client-ip-addresses/)
* [The perils of the “real” client IP](https://adam-p.ca/blog/2022/03/x-forwarded-for/)
* [Security 101: X-Forwarded-For vs. Forwarded vs PROXY](https://systemoverlord.com/2020/03/25/security-101-x-forwarded-for-vs-forwarded-vs-proxy.html)
* https://ipinfo.io/developers

