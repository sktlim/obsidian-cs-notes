> Distributed server network designed to deliver content to users more efficiently by serving data from the server closest to the user's location
## Philosophy
### Enter Deep
- **Enter deep** into access network of ISPs by deploying server clusters in access ISPs all over the world
- Get closer to end users to reduce perceived delay and increase throughput 

### Bring Home
- Bring ISPs home by building large clusters at smaller number of sites
- CDNs place cluster inside IXPs

## CDN Operation
Most CDNs take advantage of DNS to intercept and redirect requests

1) User host's Local DNS (LDNS) sends DNS query to an authoritative name server for requested website
2) Authoritative DNS returns a hostname for CDN's domain to LDNS
3) LDNS sends query for CDN's domain and DNS query enters into CDN's private DNS infrastructure 
4) LDNS forwards IP address of CDN to user's host
5) User host TCP with CDN and get file

## Cluster Selection Strategy
### Geographically Closest
- Select closest CDN based on LDNS's IP address from DNS query

### Real-time measurements
- CDNs can measure delay and loss performance between cluster and client by pinging client
- However, many LDNS aren't configured to handle these probes 