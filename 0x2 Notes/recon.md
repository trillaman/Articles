## Reconnaissance notes for bug bounty
This notes comes up to systematize knowledge I get by doing TryHackMe and HackTheBox. Maybe first part of recon is not very useful in CTFs like this, but I also want to try myself in bug bounties so this is made to become handy notes when doing stuff.  

## Step 1 - Searching for subdomains and IPs

### AMASS Intel  
Intel is for finding another TLD domains that are somehow related to domain we are doing recon on.

**For example:**

`amass intel -v -whois -d twitter.com `

**returns**
  
twitter-secured.com  
snpy.tv  
periscopecar.com                                                                                                    
summify.com                                                                                                             
twtradvice.com  
vine.ren  
twitter.ca  
twitterpublisher.net  
tweetdeck.com  
twittter.com  
twittersamvaad.com  
periscope.ren  
periscope.dating  
twiiter.com  
vncdn.co  
\\------\\  
**rest is omitted for clarity**  
  
It's not always what we want because of bug bounties got specific scope but for purpose of this notes I had to mention this.

For looking on IPs related to Organization like Twitter we can use:  
`amass intel -v -whois -org Twitter`

**ASN: 13414 - TWITTER - Twitter Inc.**  
64.63.0.0/18  
69.195.160.0/24  
69.195.162.0/23  
69.195.168.0/23  
69.195.171.0/24  
69.195.174.0/23  
69.195.185.0/24  
103.252.112.0/22  
104.244.40.0/23  
104.244.44.0/22  
185.45.5.0/24  
**ASN: 35995 - TWITTER - Twitter Inc.**  
8.25.194.0/23
185.45.4.0/24  
192.133.78.0/23  
185.45.4.0/23  
8.25.196.0/23  
\\------\\  
**rest is omitted for clarity**  

### AMASS Enum

`amass enum -ip -brute -max-depth 3 -d twitter.com -o twitter_enum.txt` 

**returns**

ss.twitter.com 199.59.150.10,199.59.149.243,199.59.148.84,199.59.148.11,199.59.150.42  
facebook3.twitter.com 199.59.148.209  
staging1-smf1-stream-data-api.twitter.com 199.59.148.141  
api-41-0-0-41-4-1.twitter.com 69.195.171.132  
sitestream-admin.twitter.com 199.59.148.137  
urls-real.api.twitter.com 199.16.156.113    
\\------\\  
**rest is omitted for clarity**  
  
### AMASS DB  
If we want to query built-in amass db we can also put output to file:  
`amass db -show -ip -d twitter.com > twitter.output`  

This will get us grepable result we can filter out IPs from.  
  
### Grepping IPs
To get only IPs for further scanning (for example with nmap) we can grep this with:    
`grep -E -o "([0-9]{1,3}[\.]){3}[0-9]{1,3}" twitter.output`  
This will result with:  
  
199.59.150.10  
199.59.149.243  
199.59.148.84  
199.59.148.11  
199.59.150.42  
199.59.148.209  
199.59.148.141  
69.195.171.132  
199.59.148.137  
199.16.156.113  
\\------\\  
**rest is omitted for clarity** 


## Step 2 - Scanning with nmap  
### NMAP  
