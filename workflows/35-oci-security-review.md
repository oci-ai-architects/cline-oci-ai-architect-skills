# Workflow: OCI Security Review

## Purpose
Security hardening checklist for OCI AI workloads.

## Steps

### 1. IAM Review

- [ ] **Compartment isolation**: AI resources in dedicated compartment
- [ ] **Least privilege policies**: Minimal permissions per group/dynamic-group
- [ ] **Dynamic groups**: For instance principals (no API keys on instances)
- [ ] **MFA enabled**: For all admin users
- [ ] **Tag-based access**: Cost center and environment tags

```
# Example: Restrict GenAI to AI team only
Allow group AIArchitects to manage generative-ai-family in compartment ai-project
Allow group AIArchitects to read all-resources in compartment ai-project

# Dynamic group for compute instances
Allow dynamic-group AIWorkloads to use generative-ai-family in compartment ai-project
```

### 2. Network Security

- [ ] **Private subnets**: All AI workloads in private subnets
- [ ] **Service gateway**: Access OCI services without internet
- [ ] **Network security groups**: Restrict ingress/egress per workload
- [ ] **No public IPs**: On databases, AI endpoints, or GPU instances
- [ ] **WAF**: On any public-facing load balancers
- [ ] **DRG**: For multi-VCN or hybrid connectivity

### 3. Data Protection

- [ ] **Encryption at rest**: Customer-managed keys (OCI Vault)
- [ ] **Encryption in transit**: TLS 1.3 for all connections
- [ ] **Object Storage**: Private bucket, no public access
- [ ] **Database**: TDE enabled, network encryption
- [ ] **Secrets**: Stored in OCI Vault, rotated regularly
- [ ] **PII handling**: Identified, classified, and protected

### 4. AI-Specific Security

- [ ] **Model access**: Restricted to authorized endpoints
- [ ] **Prompt injection**: Input validation and sanitization
- [ ] **Output filtering**: PII/sensitive data scrubbing
- [ ] **Audit logging**: All GenAI API calls logged
- [ ] **Data sovereignty**: Models deployed in correct region
- [ ] **Training data**: Access-controlled, encrypted, versioned

### 5. Monitoring & Detection

- [ ] **Cloud Guard**: Enabled with AI-relevant detector recipes
- [ ] **OCI Logging**: Audit logs retained for compliance period
- [ ] **Alarms**: Set for unusual token usage, error spikes, latency
- [ ] **Security Zones**: Enforce policies on AI compartments
- [ ] **Vulnerability scanning**: On OKE nodes and container images

### 6. Compliance Checklist

| Requirement | OCI Service | Status |
|-------------|-------------|--------|
| Data residency | Region selection | [ ] |
| Encryption at rest | OCI Vault + TDE | [ ] |
| Encryption in transit | TLS 1.3 | [ ] |
| Access control | IAM + NSG | [ ] |
| Audit trail | Audit Service | [ ] |
| Incident response | Cloud Guard | [ ] |

## Output
Document findings in a security review report with:
1. Current state assessment
2. Findings (Critical / High / Medium / Low)
3. Remediation steps with timeline
4. Residual risk acceptance
