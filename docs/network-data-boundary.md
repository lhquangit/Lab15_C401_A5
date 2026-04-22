# Network & Data Boundary

This document defines the security boundaries and data handling policies for the **Hospital Clinical Support Assistant**.

## 1. Security Zones

### 1.1 Trusted Zone (On-Premise)
*   **Contents:** HIS/EMR Databases, Patient Identity (PII/PHI), LDAP/AD.
*   **Access Policy:** No direct inbound access from the Public Internet. All access must originate from the Cloud Orchestrator via the secure VPN/ExpressRoute tunnel.
*   **Data Handling:** Data is only retrieved in "read-only" mode for context augmentation.

### 1.2 Managed Zone (Public Cloud VNet)
*   **Contents:** Application API, Vector Database, Redis Cache.
*   **Access Policy:** Restricted to authenticated internal users. Protected by Azure WAF and IP Whitelisting (Hospital IP ranges).
*   **Data Handling:** Temporary storage of chat history (encrypted). No persistent storage of full medical records.

### 1.3 Third-Party Zone (LLM Provider)
*   **Provider:** Azure OpenAI Service.
*   **Policy:** Data is processed for inference but **not used to train** the base models (Standard Enterprise Agreement).
*   **Data Masking:** PII scrubbing must be performed before sending context to the LLM.

## 2. Data Flow & Boundary Enforcement

1.  **Request Initiation:** User sends a query through the internal hospital network.
2.  **Authentication:** App checks user identity against the On-prem LDAP via the secure tunnel.
3.  **Context Retrieval:**
    *   **Public/SOP info:** Fetched from Cloud Vector DB.
    *   **Patient info:** Fetched from On-prem HIS via the tunnel using a secure API call.
4.  **Privacy Scrubbing:** A dedicated middleware identifies and masks sensitive PII (Names, IDs) before creating the final LLM prompt.
5.  **Inference:** The scrubbed prompt is sent to Azure OpenAI via an encrypted backbone network (not public internet).
6.  **Response Delivery:** The response is sent back to the user and logged in the Audit Trail.

## 3. Encryption Standards
*   **In Transit:** TLS 1.3 / SSL with high-grade ciphers.
*   **At Rest:** AES-256 for Database, Object Storage, and Vector Index.
*   **Key Management:** Azure Key Vault for managing API keys, certificates, and encryption keys.
