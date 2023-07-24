# Security Best Practices

## MDE Data location
Ou se situe les données at-rest de intune (endpoint.microsoft)?

Les données d'Intune, c'est à dire les éléments de configuration ne sont pas soumis au chiffrement BYOK avec les clé Customer Key, Ref : https://learn.microsoft.com/en-us/microsoft-365/compliance/customer-key-overview?view=o365-worldwide#about-data-encryption-policies

Ceci pour la simple raison que ce sont des données techniques et non pas des données d'utilisateur.
 
Une grane partie de ces données 'technique' est hebergés dans la région du tenant, mais cette liste n'est pas disponible et certains service sont répliqués mondialement, comme par exemple (liste non-exhaustive) : 
- Les applications publiées : elles sont stockés sur le CDN de Microsoft, mais chiffrées avec une clé unique et donc illisible autrement (c'est pour ça d'ailleurs que nous ne pouvons pas télécharger la source d'une app déjà publiée)
- Les scripts (powershell et macOS) : ils sont stockés sur le CDN de Microsoft, même comportement et même raison que pour les app ci-dessus
Les données de diagnostique (elles sont dans le tenant Azure lié au tenant M365)
- Les audits logs de plus de 6 mois (la cible doit etre définit par l'admin du tenant, Microsoft n'a pas la main la dessus)
 
Encore plus de lecture :
- https://learn.microsoft.com/en-us/mem/intune/protect/privacy-data-store-process
- https://learn.microsoft.com/en-us/microsoft-365/enterprise/o365-data-locations?view=o365-worldwide

## Adobe AIP
Microsoft Information Protection (MIP) for Acrobat and Acrobat Reader
https://helpx.adobe.com/acrobat/kb/mip-plugin-download.html
Starting with the June 2022 release (version 22.001.20142 and later) of Acrobat and Acrobat Reader, the MIP plugin is bundled with the installer. It does not require a separate installation.