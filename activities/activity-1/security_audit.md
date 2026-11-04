
# Security Audit: Raw Data Landing Zone

## High-Level Context
The system allows an external vendor to upload data into cloud storage, which is then used by downstream processes such as machine learning, data science and ETL. However, the current architecture has several security vulnerabilities that could expose sensitive data.
## Vulnerabilities Identified
### Vulnerability 1: Public Storage Access
**Location:** Public storage container (raw-insecure).
**Risk:** Public access is enabled, meaning anyone with the relevant URL could potentially read stored files without authentication. This could lead to sensitive information being exposed.
**Severity:** High

### Vulnerability 2: Hardcoded SAS Token
**Location:** Script used to access the storage container.
**Risk:** The SAS token is hardcoded into the script and has excessive permissions, including read, write and delete. It also expires in 2099. If someone obtains the token, they could access, modify or delete data without authorisation.
**Severity:** Critical 

### Vulnerability 3: Outdated TLS Encryption
**Location:** HTTPS connection between the external vendor and cloud storage.
**Risk:** The connection uses TLS 1.0, which is outdated and has known security weaknesses. This increases the risk of data being intercepted or compromised during transmission.
**Severity:** High

## Recommendations
1. Disable public access to the storage container and only allow authorised users and services to access the data.
2. Remove the hardcoded SAS token and use managed identities where possible. If SAS tokens are needed, they should have limited permissions and short expiry times.
3. Upgrade TLS 1.0 to TLS 1.2 or higher to improve security when transferring data.
4. Enable logging and monitoring to identify suspicious access or activity.
5. Apply the principle of least privilege so users and services only have the permissions they need.
