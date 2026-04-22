# Infrastructure Options Comparison

This document evaluates the different infrastructure deployment models for the **Hospital Clinical Support Assistant** project.

## 1. Comparison Matrix

| Criteria | Cloud (Public) | On-Premise | Hybrid (Proposed) |
| :--- | :--- | :--- | :--- |
| **Security (Data Privacy)** | Medium - Relies on provider compliance (HIPAA/GDPR). | **High** - Full control over data residency. | **High** - Sensitive data (PHI/PII) stays On-prem. |
| **Cost (OpEx vs CapEx)** | Low (Pay-as-you-go), no upfront hardware cost. | High Upfront (Servers, GPUs, Cooling). | Balanced - OpEx for AI, utilizing existing On-prem servers. |
| **Deployment Speed** | **Very Fast** - Managed services ready in minutes. | Slow - Hardware procurement and setup. | Medium - Fast for AI components, slower for networking. |
| **AI Capabilities** | **High** - Instant access to latest LLMs (GPT-4, etc.). | Low - Requires massive investment in GPUs. | **High** - Access to Cloud LLMs via API. |
| **Scalability** | **Unlimited** - Automatic scaling on demand. | Hard - Limited by physical hardware capacity. | **High** - Cloud components scale elastically. |
| **Operational Complexity** | Low - Managed by provider. | High - In-house maintenance required. | Medium - Requires management of Cloud-to-On-prem link. |

## 2. Selection Rationale: Hybrid Deployment

The **Hybrid Model** is selected as the optimal choice for the following reasons:

1.  **Compliance & Trust**: Hospitals are strictly regulated regarding Patient Health Information (PHI). Keeping the primary database on-premise ensures compliance with local data sovereignty laws.
2.  **Cost Efficiency**: Building an on-premise infrastructure capable of running modern LLMs (like GPT-4) requires significant investment in NVIDIA A100/H100 GPUs. Utilizing Cloud APIs (Azure OpenAI) is significantly cheaper for MVP.
3.  **Performance**: By using a Cloud provider with a region in Singapore (like Azure), we maintain low latency for the AI processing while the data retrieval remains fast within the local network.
4.  **Future Proofing**: The hybrid approach allows us to easily switch between different Cloud LLM providers or eventually move parts of the workload on-premise if local AI models become efficient enough.

## 3. Component Distribution

*   **Public Cloud (Azure Southeast Asia):**
    *   AI Model Inference (GPT-4o, Embedding models).
    *   Application Backend (API Orchestration, Business Logic).
    *   Vector Database (for SOPs and public medical knowledge).
    *   Observability Stack (Azure Monitor, Log Analytics).
*   **On-Premise (Hospital Data Center):**
    *   Patient Medical Records (HIS/EMR Database).
    *   Identity & Access Management (Active Directory/LDAP).
    *   Data Connectors (ETL processes).
    *   Local Security Gateway.
