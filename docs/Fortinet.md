# Fortinet

```title="Desactiver les vieux TLS ==> For HTTPS"
config sys global  
set strong-crypto enable  
set admin-https-ssl-versions tlsv1-2 tlsv1-3   
end
``` 
```title="Desactiver les vieux TLS ==> For SSL-VPN"  
config vpn ssl settings  
set ssl-min-proto-ver tls1-2  
set ssl-max-proto-ver tls1-3    
end
```

```title="Require TLS 1.2 for HTTPS administrator access to the GUI"
config system global
set admin-https-ssl-versions tlsv1-2
end
TLS 1.2 is currently the most secure SSL/TLS supported version for SSL-encrypted administrator access.
```
```title="Re-direct HTTP GUI logins to HTTPS. Go to System > Settings > Administrator Settings and enable Redirect to HTTPS to make sure that all
attempted HTTP login connections are redirected to HTTPS."
config system global
set admin-https-redirect enable
end
```