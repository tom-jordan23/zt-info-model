# Cross-Pillar Overview

### Cross-Pillar Integration Diagram

This diagram illustrates how each CISA Zero Trust Pillar integrates with others through a shared integration infrastructure layer. It emphasizes information flow and decision-making across the architecture.

Each pillar is enclosed in a subgraph and highlights:

- Major internal components
- Inputs and outputs
- Connection to shared APIs and event brokers

The Shared Integration Infrastructure enables:
- Real-time identity and posture evaluation
- Attribute- and risk-based access decisions
- Coordinated alerting and response
- Vendor-neutral event-driven orchestration

## Cross-Pillar Information Model

```mermaid
flowchart LR

%% Shared Integration Infrastructure
SharedInfra["Common API & Event Infrastructure"]

%% Identity Pillar
subgraph Identity [Identity Pillar]
  IdP["Identity Provider"]
  MFA["MFA Broker"]
  RiskEngine["Risk Engine"]
  IdentityPDP["Policy Decision Point"]
  ID_Input["Device Context, Threat Intel"]
  ID_Output["OIDC Tokens, Access Decisions"]
end
ID_Input --> RiskEngine
IdP --> MFA
MFA --> IdentityPDP
RiskEngine --> IdentityPDP
IdentityPDP --> ID_Output
IdP --- SharedInfra
RiskEngine --- SharedInfra
IdentityPDP --- SharedInfra

%% Device Pillar
subgraph Device [Device Pillar]
  MDM["MDM / EDR"]
  Inventory["Device Inventory"]
  Posture["Posture Assessment"]
  DevicePDP["Policy Decision Point"]
  DV_Input["Threat Intel, Asset Metadata"]
  DV_Output["Trust Score, Certs"]
end
Inventory --> Posture
MDM --> Posture
Posture --> DevicePDP
DV_Input --> MDM
DevicePDP --> DV_Output
MDM --- SharedInfra
DevicePDP --- SharedInfra

%% Network Pillar
subgraph Network [Network Pillar]
  SDN["SDN"]
  SEG["Secure Edge Gateway"]
  Microseg["Microsegmentation"]
  NetPDP["Policy Decision Point"]
  FlowMonitor["Flow Monitor"]
  NT_Input["Identity/Device Context, Threats"]
  NT_Output["Segmentation, Alerts"]
end
SDN --> Microseg
SEG --> Microseg
Microseg --> NetPDP
FlowMonitor --> NetPDP
NT_Input --> NetPDP
NetPDP --> NT_Output
NetPDP --- SharedInfra
FlowMonitor --- SharedInfra

%% Application Pillar
subgraph App [Applications & Workloads Pillar]
  AppCatalog["App Registry"]
  Broker["Access Broker"]
  WAF["WAF"]
  Runtime["Runtime Security"]
  AppPDP["Policy Decision Point"]
  AP_Input["Identity, Risk, Device"]
  AP_Output["App Tokens, Alerts"]
end
AppCatalog --> Broker
Broker --> WAF
Runtime --> AppPDP
AP_Input --> AppPDP
AppPDP --> AP_Output
AppPDP --- SharedInfra
Broker --- SharedInfra

%% Data Pillar
subgraph Data [Data Pillar]
  Classifier["Data Classifier"]
  Tagger["Tagging"]
  DataPDP["Policy Engine"]
  DLP["DLP"]
  Encryptor["Encryptor"]
  Storage["Storage Enforcement"]
  DA_Input["Access Context, Claims"]
  DA_Output["Tags, Encryption Status, DLP Events"]
end
Classifier --> Tagger
Tagger --> DataPDP
DataPDP --> DLP
DataPDP --> Encryptor
DataPDP --> Storage
DA_Input --> DataPDP
DataPDP --> DA_Output
DataPDP --- SharedInfra
DLP --- SharedInfra
Storage --- SharedInfra
```
<br><br>
## References

- [CISA Zero Trust Maturity Model v2.0 – All Pillars](https://www.cisa.gov/resources-tools/resources/zero-trust-maturity-model)
- [NIST SP 800-207 – Zero Trust Architecture](https://csrc.nist.gov/publications/detail/sp/800-207/final)


