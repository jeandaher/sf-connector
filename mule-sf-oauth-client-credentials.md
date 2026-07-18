## SF connector - Implementation dans Mule App  - Methode OAuth Client Credentials

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

Fichier: dev.yaml    
```
# Experience layer
http:
  listener:
    host: "0.0.0.0"
    port: "8081"
# System layer - connexion Salesforce
salesforce:
  loginUrl: "https://myorg-dev-ed.develop.my.salesforce.com"
# Process layer - regles metier par defaut
case:
  defaultOrigin: "Web"
  defaultStatus: "New"
```

Example de sub flow Select Case by Id:    
```

  <sub-flow name="system-salesforce-get-case-by-id" doc:name="system-salesforce-get-case-by-id">
    <try doc:name="Try">
      <salesforce:query config-ref="Salesforce_Config" doc:name="SOQL SELECT Case by Id">
        <salesforce:salesforce-query>SELECT Id, CaseNumber, RecordTypeId, Subject, Description, Status, Origin, Priority, AccountId, ContactId, SuppliedEmail, SuppliedName, SuppliedPhone, CreatedDate, LastModifiedDate FROM Case WHERE Id = ':caseId'</salesforce:salesforce-query>
        <salesforce:parameters><![CDATA[#[{caseId: vars.caseId}]]]></salesforce:parameters>
      </salesforce:query>
      <choice doc:name="Case trouve ?">
        <when expression="#[isEmpty(payload)]">
          <raise-error type="SYSTEM:NOT_FOUND" description="#['Case ' ++ (vars.caseId as String) ++ ' introuvable dans Salesforce.']" doc:name="Raise SYSTEM:NOT_FOUND" />
        </when>
        <otherwise>
          <ee:transform doc:name="Mapper vers le contrat System (camelCase)">
            <ee:message>
              <ee:set-payload><![CDATA[%dw 2.0
output application/json
var c = payload[0]
---
{
  id: c.Id,
  caseNumber: c.CaseNumber,
  recordTypeId: c.RecordTypeId,
  subject: c.Subject,
  description: c.Description,
  status: c.Status,
  origin: c.Origin,
  priority: c.Priority,
  accountId: c.AccountId,
  contactId: c.ContactId,
  suppliedEmail: c.SuppliedEmail,
  suppliedName: c.SuppliedName,
  suppliedPhone: c.SuppliedPhone,
  createdDate: c.CreatedDate,
  lastModifiedDate: c.LastModifiedDate
}]]></ee:set-payload>
            </ee:message>
          </ee:transform>
        </otherwise>
      </choice>
      <error-handler>
        <on-error-continue type="SALESFORCE:CONNECTIVITY" doc:name="Mapper vers SYSTEM:CONNECTIVITY">
          <raise-error type="SYSTEM:CONNECTIVITY" description="#[error.description default 'Echec de l\'authentification Salesforce.']" doc:name="Raise SYSTEM:CONNECTIVITY" />
        </on-error-continue>
        <on-error-continue type="SALESFORCE:*" doc:name="Mapper vers SYSTEM:ERROR">
          <raise-error type="SYSTEM:ERROR" description="#['Echec de la lecture du Case ' ++ (vars.caseId as String) ++ ' dans Salesforce.']" doc:name="Raise SYSTEM:ERROR" />
        </on-error-continue>
      </error-handler>
    </try>
  </sub-flow>
```
