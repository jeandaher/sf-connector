## SF connector - Implementation dans Mule App  - Methode OAuth JWT

Fichier - dev-secrets.properties:    
```
salesforce.jwt.keyStorePassword=
```

Fichier - dev.yaml:    
```
# Experience layer
http:
  listener:
    host: "0.0.0.0"
    port: "8081"
# System layer - connexion Salesforce (JWT Bearer Flow)
salesforce:
  loginUrl: "https://myorg-dev-ed.develop.my.salesforce.com"
  jwt:
    username: "integration.user@edition10.com"
    consumerKey: "3MxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxbO9"
    keyStorePath: "keys/salesforce-jwt-keystore.p12"
    certificateAlias: "facetagent-jwt"    
    audienceUrl: "https://login.salesforce.com"
# Process layer - regles metier par defaut
case:
  defaultOrigin: "Web"
  defaultStatus: "New"
```

Sfdc-config:    
```
  <salesforce:sfdc-config name="Salesforce_Config" doc:name="Salesforce Config">
    <salesforce:jwt-connection
      consumerKey="#[p('salesforce.jwt.consumerKey')]"
      principal="#[p('salesforce.jwt.username')]"
      keyStore="#[p('salesforce.jwt.keyStorePath')]"
      storePassword="#[p('secure::salesforce.jwt.keyStorePassword')]"
      certificateAlias="#[p('salesforce.jwt.certificateAlias')]"
      tokenEndpoint="#[p('salesforce.loginUrl') ++ '/services/oauth2/token']"
      audienceUrl="#[p('salesforce.jwt.audienceUrl')]" />
  </salesforce:sfdc-config>
```

Les sub flow ne changent pas, seulement la configuration de la connection 

