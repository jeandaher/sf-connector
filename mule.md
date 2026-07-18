## SF connector - Implementation dans Mule App  - Methode 1 M2M  

Sfdc-config:    
```
  <salesforce:sfdc-config name="Salesforce_Config" doc:name="Salesforce Config">
    <salesforce:oauth-client-credentials-connection>
      <salesforce:oauth-client-credentials
        clientId="#[p('secure::salesforce.clientId')]"
        clientSecret="#[p('secure::salesforce.clientSecret')]"
        tokenUrl="#[p('salesforce.loginUrl') ++ '/services/oauth2/token']" />
    </salesforce:oauth-client-credentials-connection>
  </salesforce:sfdc-config>
```

