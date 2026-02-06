# Workflow: OCI AI Project Setup

## Purpose
Initialize a new OCI project with proper compartment structure, networking, and AI service access.

## Steps

### 1. Define Project Scope
- Identify workload type: GenAI inference, RAG, fine-tuning, multi-agent
- Determine data residency requirements (EU regions for Cohere models)
- Estimate GPU needs (A10 for inference, A100/H100 for training)

### 2. Create Compartment Structure
```hcl
# Terraform - Compartment hierarchy
resource "oci_identity_compartment" "ai_project" {
  compartment_id = var.tenancy_ocid
  name           = "ai-project-${var.project_name}"
  description    = "AI project compartment"
}

resource "oci_identity_compartment" "ai_compute" {
  compartment_id = oci_identity_compartment.ai_project.id
  name           = "compute"
}

resource "oci_identity_compartment" "ai_data" {
  compartment_id = oci_identity_compartment.ai_project.id
  name           = "data"
}
```

### 3. Configure Networking
```hcl
resource "oci_core_vcn" "ai_vcn" {
  compartment_id = oci_identity_compartment.ai_project.id
  cidr_blocks    = ["10.0.0.0/16"]
  display_name   = "ai-vcn"
  dns_label      = "aivcn"
}

# Private subnet for AI workloads
resource "oci_core_subnet" "ai_private" {
  compartment_id = oci_identity_compartment.ai_compute.id
  vcn_id         = oci_core_vcn.ai_vcn.id
  cidr_block     = "10.0.1.0/24"
  display_name   = "ai-private-subnet"
  prohibit_public_ip_on_vnic = true
}
```

### 4. Set Up IAM Policies
```
Allow group AIArchitects to manage generative-ai-family in compartment ai-project
Allow group AIArchitects to manage data-science-family in compartment ai-project
Allow group AIArchitects to use virtual-network-family in compartment ai-project
Allow dynamic-group AIAgents to use generative-ai-family in compartment ai-project
```

### 5. Enable AI Services
- [ ] Request GPU quota (if needed) via support ticket
- [ ] Enable OCI GenAI in target region
- [ ] Set up Object Storage for training data
- [ ] Configure OKE if using AI Blueprints

### 6. Verify Setup
```bash
# Check GenAI access
oci generative-ai model list --compartment-id $COMPARTMENT_ID

# Check GPU quota
oci limits value list --service-name compute --compartment-id $TENANCY_ID \
  --scope-type REGION --query "data[?contains(name, 'gpu')]"
```

## Decision Points
- **Managed vs Self-Hosted**: Use OCI GenAI Service for managed, AI Blueprints for self-hosted
- **Shared vs Dedicated**: On-demand endpoints for dev, Dedicated AI Clusters for production
- **Region Selection**: Frankfurt/Amsterdam for EU data residency with Cohere models
