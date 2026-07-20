## Create the External Client App 

### 
Dans Basic Information    
Enter the name : ECA MuleSoft JWt

Dans API (Enable OAuth Settings):
☑ Enable OAuth Settings

Dans App Settings  
Sasir le url suivant dans Callback URL (ne sera pas utilisé , mais c est un champ obligatoire):      
https://login.salesforce.com/services/oauth2/callback    

Dans OAuth Scopes (minimum)    
Choisir les scopes suivant:    
Access the identity URL service(id, profile, email, address, phone)    
Manage user data via APIs (api)
Perform requests at any time (refresh_token, offline_access)    
Access unique user identifiers(openid)    

👉 Ne mets pas full en production.    


Dans Flow Enablement    
Enable seulement :
☑ Enable JWT Bearer Flow
 Les autres non:

* Non Enable Client Credentials Flow
* Non Enable Authorization Code and Credentials Flow
* Non Enable Device Flow
* OUI Enable JWT Bearer Flow
* Non Enable Token Exchange Flow

Avec Enable JWT Bearer Flow , un bouton est disponible pour telecharger le certificat public:
salesforce-jwt-public.crt    

Appuyer sur le boutton "Create",    
Sauvegarder l'External Client App.      


Aller dans l'External Client App    
Va dans l'onglet "Policies" et cliquer sur "Edit"     
Dans OAuth Policies / Plugin Policies , Permitted Users: 
select :  Admin approved users are pre-authorized     

### Utilisateur et  permissionSet

Creer un permissionSet facetagentPerm
Creer un utilisateur: integration-facetagent@example.com   
Ajouter l'utilisateur à la permissionSet    

Dans l'external Client App ,     
aller dans policies , Edit, puis selectionner la permissionSet facetagentPerm et l'ajouter.    



