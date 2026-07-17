## 🧩 1. Les 4 méthodes d’authentification Salesforce (vision globale)
### 1️⃣ Username + Password + Security Token
<b>À éviter.</b>
* Risque de rotation du token    
* Pas de scopes OAuth    
* Pas de révocation propre    
* Pas de séparation des permissions    
* Dépend d’un utilisateur humain    
* Incompatible MFA moderne    
    
### 2️⃣ OAuth 2.0 Web Server Flow (Authorization Code)
<b>Conçu pour les applications web.</b>   
* Nécessite un redirect URI    
* Nécessite un consent screen    
* Nécessite un refresh token    
* Permet des scopes OAuth    
* Permet une révocation propre    
* <b>Méthode utilisée par Informatica</b>    

### 3️⃣ OAuth 2.0 Client Credentials Flow    
<b>Simple, efficace, M2M.</b>    
* MuleSoft envoie clientId + clientSecret    
* Salesforce renvoie un access_token    
* Pas de refresh token    
* Pas de mot de passe Salesforce     
* Pas de certificat    
* Pas d’utilisateur humain    
* <b>Machine‑to‑machine (M2M)</b>    
* <b>Recommandé pour MuleSoft (si pas besoin de JWT)</b>    

### 4️⃣ OAuth 2.0 JWT Bearer Flow
<b>Le plus sécurisé.</b>    
* Pas de mot de passe     
* Pas de token Salesforce à stocker    
* Authentification serveur‑à‑serveur    
* Rotation simple    
* Permissions contrôlées via External Client App    
* Certificat signé    
* <b>Méthode enterprise‑grade</b>    
* <b>Méthode recommandée pour MuleSoft</b>    

### 🧱 2. Tableau comparatif des 4 méthodes
| Méthode             | Sécurité     | M2M     | Refresh Token | Certificat | Usage typique |
| ------------------- | ------------ | ------- | ------------- |----------- | ------------- |
| Username + Password | ❌ Faible   |  ❌     |  ❌          | ❌         | Débogage local |
| OAuth Web Server (Auth Code) | ⭐⭐⭐⭐ |  ❌     |  ✔          | ❌         | Apps web, IICS |
| OAuth Client Credentials | ⭐⭐⭐   |  ✔    |  ❌          | ❌         | MuleSoft simple |
| OAuth JWT Bearer | ⭐⭐⭐⭐⭐ |  ✔    |  ✔(via Connected App)| ✔        | MuleSoft enterprise |

---

### 🐴 3. Méthodes compatibles MuleSoft (Anypoint Platform)
#### ✔ Méthodes réellement utilisées en production MuleSoft

| Méthode             | Support MuleSoft | Recommandation   | 
| ------------------- | ------------ | ------- |
| Username + Password | ✔  |  ❌ Déconseillé |  
| OAuth Web Server Flow | ✔  |  ❌ Pas adapté (nécessite consent screen) |  
|OAuth Client Credentials | ✔  | ⭐ Recommandé |  
|OAuth JWT Bearer | ✔  | ⭐⭐⭐⭐ Recommandé (enterprise-grade) |  

---

### 🔥 Pourquoi MuleSoft préfère Client Credentials ou JWT ?
* Pas d’utilisateur humain    
* Pas de consent screen    
* Pas de redirect URI    
* Pas de refresh token à gérer (Client Credentials)    
* Certificat sécurisé (JWT)    
* Rotation simple    
* Compatible avec External Client App Manager    
* Parfait pour API-led connectivity        

👉 <b>Ton architecture MuleSoft Case CRUD doit utiliser JWT Bearer (idéal) ou Client Credentials (simple).</b>    

--- 

### 🧩 4. Méthodes compatibles Informatica (IICS / IDMC)
#### ✔ Méthode supportée par le connecteur Salesforce d’Informatica    

| Méthode             | Support IICS | Recommandation   | 
| ------------------- | ------------ | ------- |
| Username + Password | ✔  |  ❌ Déconseillé |  
| OAuth Web Server Flow (Authorization Code) | ✔  | ⭐⭐⭐⭐⭐ Méthode officielle |  
|OAuth Client Credentials | ❌  | — |  
|OAuth JWT Bearer | ❌ | — |  


#### 🔍 Pourquoi Informatica impose OAuth Authorization Code ?    
* Le Secure Agent doit se reconnecter automatiquement    
* Salesforce n’autorise pas l’usage d’un access_token seul pour des intégrations persistantes    
* Le connecteur IICS gère automatiquement le refresh token    
* Le flow Authorization Code est le seul qui fournit un refresh token utilisable par un agent autonome     
* Le consent screen est donné une fois lors de la création de la connexion    
* Le redirect URI est celui d’Informatica    

👉 <b>Donc Informatica = OAuth Authorization Code + Refresh Token obligatoire.</b>    

--- 

### 🏆 Recommandations finales pour ton architecture
#### 🔧 Pour MuleSoft (API Case CRUD)    
👉 OAuth 2.0 JWT Bearer Flow    
* Certificat
* External Client App
* Très sécurisé
* Rotation simple
* Parfait pour API-led
👉 Alternative : <b>OAuth Client Credentials</b>
* Plus simple
* Suffisant si pas besoin de certificat

#### 🔧 Pour Informatica (SDO / Data Sync / Data Integration)
👉 OAuth 2.0 Authorization Code Flow
* Refresh token obligatoire
* Consent screen initial
* Reconnexion automatique du Secure Agent
* Méthode officielle du connecteur Salesforce





Méthode officielle du connecteur Salesforce
