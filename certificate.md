
### Generate the certificate for Salesforce / MuleSoft 

#### ✔ Étape 1 — Générer un keystore PKCS12

Avec Powershell window:    
Aller vers le repertoire : C:\src\mule\certs\salesforce
Generer le ceritificate pour l'environnement de dev avec la commande: 

```
openssl req -x509 -newkey rsa:2048 -nodes -keyout salesforce-jwt-private.key  -out salesforce-jwt-public.crt -days 365 -subj "/CN=MuleSoft Integration"
```

si openssl n'existe pas , on a keytool dans powershell qui peut generer le certificate avec:

```
keytool -genkeypair -alias facetagent-jwt -keyalg RSA  -keysize 2048 -validity 365 -storetype PKCS12 -keystore salesforce-jwt-keystore.p12 -storepass <<password>> -dname "CN=MuleSoft Integration"
```

Génération d'une paire de clés RSA de 2?048 bits et d'un certificat auto-signé (SHA256withRSA) d'une validité de 365 jours    
        pour : CN=MuleSoft Integration    
    
le resultat est : salesforce-jwt-keystore.p12    
👉 Ce fichier est directement utilisable par MuleSoft      
👉 Il contient la clé privée + le certificat public    

---

#### ✔ Étape 2 — Extraire le certificat public (à uploader dans Salesforce)

```
keytool -exportcert -alias facetagent-jwt -keystore salesforce-jwt-keystore.p12 -storetype PKCS12 -storepass <<password>> -rfc -file salesforce-jwt-public.crt
```

le resultat est : salesforce-jwt-public.crt       
👉 C’est ce fichier que tu uploades dans Salesforce → External Client App → Digital Signatures.    


#### ✔ Étape 3 — Vérifier le keystore

```
keytool -list -keystore salesforce-jwt-keystore.p12 -storetype PKCS12
```

le resultat est : l'affichage de la clé d'acces et le Alias name qui est:  facetagent-jwt    

👉  ca montre que la clé est valide     


#### ✔ Étape 4 — Déplacer les fichiers au bon endroit    

Mettre le certificate sur ton disque (hors projet) , example : \mule\certs\salesforce


#### ✔ Étape 5 — mettre nom et alias du certificate dans les variables de l'environnement dev.yaml (uat.yaml/prod.yaml).    

```
salesforce:
  jwt:
    username: "integration.facetagent@yourorg-dev-ed.com"
    consumerKey: "A_REMPLACER_avec_le_Consumer_Key_du_Connected_App_dev"
    keyStorePath: "keys/salesforce-jwt-keystore.p12"
    certificateAlias: "facetagent-jwt"
```

--- 

## ⭐ 1. Où mettre la licence MuleSoft ?

### On-Promises     

Mettre le fichier de licence dans :     
$MULE_HOME/conf/mule-license.key

Copier le fichier salesforce-jwt-keystore.p12 
vers  <ton-projet>/src/main/resources/keys/salesforce-jwt-keystore.p12

si vous metter dans dev.yaml
 keyStorePath: "keys/salesforce-jwt-keystore.p12"

### ClouHub    

