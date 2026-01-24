# AWS MCP Security - Hướng Dẫn Bảo Mật Toàn Diện

## Giới Thiệu

Model Context Protocol (MCP) do Anthropic giới thiệu (11/2024) là giao thức mở chuẩn hóa kết nối giữa AI systems và external tools, data sources, services. Tài liệu này cung cấp hướng dẫn toàn diện về bảo mật khi triển khai MCP trên AWS, tận dụng Amazon Bedrock, Bedrock AgentCore và AWS security ecosystem.

**Tài liệu tham khảo:**
- [MCP Specification](https://modelcontextprotocol.io/)
- [Amazon Bedrock](https://docs.aws.amazon.com/bedrock/)
- [AWS MCP Servers](https://github.com/awslabs/mcp)

---

## 1. Identity & Access Management

### AWS IAM

AWS IAM là nền tảng bảo mật MCP, kiểm soát ai có thể invoke AI agents và agents có thể access tài nguyên nào.

**Best Practices:**
- Implement least privilege principle
- Tạo separate roles cho different agent types
- Sử dụng IAM permission boundaries
- Regular review với IAM Access Analyzer

**Tài liệu:** [AWS IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

### Amazon Bedrock AgentCore Identity

Purpose-built identity management cho AI agents, cung cấp secure authentication cho user-invoked agents và agents với pre-authorized consent.

**Tính năng:**
- Dual authentication model (user-invoked vs pre-authorized)
- Secure token vault
- Native AWS Secrets Manager integration
- Enterprise identity provider integration

**Lợi ích:**
- Không cần user migration
- Extend existing security policies to AI agents
- Consistent governance across traditional và AI-driven systems

**Tài liệu:** [Amazon Bedrock Agents](https://docs.aws.amazon.com/bedrock/latest/userguide/agents.html)

### OAuth 2.0 Authentication

MCP specification quy định OAuth 2.0 làm standard authentication mechanism cho remote MCP servers.

**Hỗ trợ:**
- Authorization code grant flow (interactive scenarios)
- OAuth Protected Resource Metadata ([RFC 9728](https://datatracker.ietf.org/doc/html/rfc9728))
- Authorization Server Metadata ([RFC 8414](https://datatracker.ietf.org/doc/html/rfc8414))
- Dynamic Client Registration
- JWT-based assertions ([RFC 7523](https://datatracker.ietf.org/doc/html/rfc7523)) cho workload-to-workload

**Tài liệu:** [OAuth 2.0 RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749)

---

## 2. Data Protection

### Encryption at Rest

Amazon Bedrock sử dụng AWS KMS để encrypt:
- Custom models
- Agents
- Knowledge base data sources
- Evaluation jobs

**Best Practices:**
- Sử dụng customer-managed keys (CMKs)
- Configure KMS key policies
- Enable automatic key rotation
- Encrypt vector stores (Amazon OpenSearch Service)
- Encrypt S3 buckets

**Tài liệu:**
- [AWS KMS](https://docs.aws.amazon.com/kms/)
- [Bedrock Encryption at Rest](https://docs.aws.amazon.com/bedrock/latest/userguide/encryption-at-rest.html)

### Encryption in Transit

- Amazon Bedrock enforces TLS 1.2 và TLS 1.3
- Cipher suites với perfect forward secrecy
- Mutual TLS (mTLS) cho bidirectional verification
- AWS Certificate Manager cho SSL/TLS certificates

**Tài liệu:** [AWS Certificate Manager](https://docs.aws.amazon.com/acm/)

### AWS Secrets Manager

Secure storage và retrieval cho credentials, API keys, connection strings.

**Tính năng:**
- Runtime credential retrieval
- Automatic rotation
- AgentCore Identity integration
- No hardcoded secrets

**Tài liệu:** [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/)

---

## 3. Network Security

### VPC Isolation

- Deploy MCP servers trong private subnets
- No direct internet access
- NAT gateways cho outbound access
- Multi-layer isolation

**Tài liệu:**
- [Amazon VPC](https://docs.aws.amazon.com/vpc/)
- [VPC Security Best Practices](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-best-practices.html)

### AWS PrivateLink

Private connectivity đến Amazon Bedrock services.

**Lợi ích:**
- Traffic không traverse public internet
- VPC interface endpoints
- Endpoint policies cho access control

**Tài liệu:** [AWS PrivateLink](https://docs.aws.amazon.com/vpc/latest/privatelink/)

### Security Groups & Network ACLs

**Security Groups (Stateful):**
- Instance-level firewall
- Least privilege rules

**Network ACLs (Stateless):**
- Subnet-level filtering
- Additional defense layer

**Tài liệu:**
- [Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html)
- [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)

### CloudFront & AWS WAF

**CloudFront:**
- HTTPS encryption (HTTP/2, HTTP/3)
- DDoS protection

**AWS WAF:**
- Web exploit protection
- Rate limiting
- Prompt injection filtering

**Tài liệu:**
- [Amazon CloudFront](https://docs.aws.amazon.com/cloudfront/)
- [AWS WAF](https://docs.aws.amazon.com/waf/)

---

## 4. Monitoring & Incident Response

### AWS CloudTrail

Capture tất cả API calls đến Amazon Bedrock.

**Capabilities:**
- Who, what, when, where tracking
- S3 log delivery
- CloudTrail Insights (ML-powered anomaly detection)
- Log file integrity validation

**Tài liệu:** [AWS CloudTrail](https://docs.aws.amazon.com/cloudtrail/)

### Amazon CloudWatch

Real-time monitoring và alerting.

**Metrics:**
- Invocation counts, latency, errors
- Throttles, token usage
- Custom metrics

**Features:**
- CloudWatch Alarms
- Invocation logging (input/output)
- SNS integration

**Tài liệu:** [Amazon CloudWatch](https://docs.aws.amazon.com/cloudwatch/)

### Amazon EventBridge

Event-driven security automation.

**Use Cases:**
- Detect sensitive operations
- Trigger Lambda remediation
- Automated credential revocation

**Tài liệu:** [Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/)

### Amazon GuardDuty

Intelligent threat detection.

**Features:**
- ML-based anomaly detection
- ECS Runtime Monitoring
- Privilege escalation detection
- Malware detection

**Tài liệu:** [Amazon GuardDuty](https://docs.aws.amazon.com/guardduty/)

---

## 5. AWS MCP Servers

AWS cung cấp reference implementations với security best practices built-in.

**Repository:** [github.com/awslabs/mcp](https://github.com/awslabs/mcp)

### AWS IAM MCP Server

Query IAM policies và permissions (read-only).

**Security:**
- ✅ Read-only access
- ✅ AWS SDK authentication
- ✅ Least privilege
- ✅ Cross-account support

**Capabilities:**
- Policy analysis
- Permission evaluation
- Access key status checking

**Link:** [IAM MCP Server](https://github.com/awslabs/mcp/tree/main/servers/iam)

### AWS Cloud Control API MCP Server

Natural language infrastructure management.

**Security:**
- ✅ Explicit user approval (mutating operations)
- ✅ CloudTrail audit logging
- ✅ Resource tagging enforcement
- ✅ Drift detection
- ✅ Resource boundaries
- ✅ Cost guardrails
- ✅ Compliance validation
- ✅ Rollback capabilities

**Link:** [Cloud Control MCP Server](https://github.com/awslabs/mcp/tree/main/servers/cloudcontrol)

**Tài liệu:** [AWS Cloud Control API](https://docs.aws.amazon.com/cloudcontrolapi/)

### AWS Serverless MCP Server

Security-first serverless management.

**Security:**
- ✅ Default read-only mode
- ✅ `--allow-write` flag required
- ✅ CloudWatch Logs restricted by default
- ✅ `--allow-sensitive-data-access` flag
- ✅ CodeGuru code scanning
- ✅ GuardDuty runtime monitoring
- ✅ Secrets Manager integration
- ✅ VPC security enforcement

**Capabilities:**
- Lambda functions
- API Gateway
- EventBridge
- Step Functions
- DynamoDB

**Link:** [Serverless MCP Server](https://github.com/awslabs/mcp/tree/main/servers/serverless)

---

## 6. Amazon Bedrock Agents với MCP

### Inline Agent SDK

Simplified MCP integration.

**Features:**
- Built-in MCP client
- Response stream parsing
- Return control flow handling
- State management
- Dynamic configuration

**Tài liệu:** [Bedrock Agents SDK](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-sdk.html)

### Return of Control (ROC)

Human-in-the-loop security pattern.

**Workflow:**
1. Agent requests sensitive action
2. Developer intercepts request
3. Present to user for approval
4. User approves/modifies parameters
5. Execution continues

**Configuration:** Action group level

**Tài liệu:** [Return of Control](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-returncontrol.html)

### User Confirmation

Simpler approval mechanism cho specific actions.

**Use Cases:**
- State-changing operations
- Sensitive resource access
- Database modifications

---

## 7. Amazon Bedrock Guardrails

Customizable safeguards đánh giá user inputs và model outputs.

### Content Filters

- Hate speech
- Violence
- Misconduct
- **Prompt attacks**

### Contextual Grounding

Minimize hallucinations bằng cách validate model responses against source content.

### Sensitive Information Filters

- PII detection và redaction
- Predefined PII types
- Custom patterns với regex

### Automated Reasoning Checks

Provably truthful responses với 99% accuracy.

**Features:**
- Mathematical logic verification
- Formal verification techniques
- Domain knowledge validation
- Hallucination prevention

**Tài liệu:**
- [Bedrock Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html)
- [Automated Reasoning](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails-automated-reasoning.html)

---

## 8. Infrastructure Security

### AWS CloudFormation

Infrastructure as Code cho MCP deployments.

**Lợi ích:**
- Consistent, repeatable deployments
- Version control
- Rollback capabilities
- Define entire MCP stack

**Tài liệu:**
- [AWS CloudFormation](https://docs.aws.amazon.com/cloudformation/)
- [CloudFormation Best Practices](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/best-practices.html)

### AWS CloudFormation Guard

Define security policies as code.

**Enforce:**
- Mandatory encryption
- Required VPC endpoints
- IAM role permission boundaries
- Logging requirements

**Link:** [CloudFormation Guard](https://github.com/aws-cloudformation/cloudformation-guard)

### AWS Config

Continuous configuration recording và compliance evaluation.

**Features:**
- Verify encryption
- Network segmentation
- Logging standards
- Automated remediation

**Tài liệu:** [AWS Config](https://docs.aws.amazon.com/config/)

---

## 9. MCP-Specific Security Controls

### Digital Signature Verification

Amazon Bedrock AgentCore Gateway:
- Verify digital signatures trên MCP servers
- Ensure chỉ trusted servers được access
- Certificate pinning
- Reject invalid certificates

### Input/Output Validation

**Input Sanitization:**
- Allowlist-based validation
- Parameterized queries
- PII detection và blocking
- Context window overflow protection

**Output Validation:**
- Validate outputs trước return
- Content security policies
- Post-invocation interceptors
- Schema conformance checking

---

## 10. Best Practices

### Least Privilege

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": [
      "bedrock:InvokeAgent",
      "bedrock:InvokeModel"
    ],
    "Resource": "arn:aws:bedrock:region:account:agent/specific-agent-id",
    "Condition": {
      "StringEquals": {
        "aws:RequestedRegion": "us-east-1"
      }
    }
  }]
}
```

### Credential Management

**Never:**
- ❌ Hardcode credentials
- ❌ Store trong environment variables
- ❌ Commit to version control
- ❌ Use long-lived access keys

**Always:**
- ✅ Use IAM roles
- ✅ Secrets Manager/Parameter Store
- ✅ Automatic rotation
- ✅ Temporary credentials (STS)

### Monitoring Checklist

- [ ] CloudTrail enabled (all regions)
- [ ] CloudWatch Logs retention configured
- [ ] VPC Flow Logs enabled
- [ ] GuardDuty activated
- [ ] Security Hub integrated
- [ ] EventBridge rules cho sensitive ops
- [ ] SNS alerts configured
- [ ] Log encryption enabled

### Audit Trail Requirements

**Record:**
- User identity
- Timestamp
- Tool invoked
- Parameters
- Approval status
- Outcome

**Protect:**
- Log file integrity validation
- Separate security account
- Immutable storage (S3 Object Lock)
- Encryption at rest

**Tài liệu:** [Amazon Detective](https://docs.aws.amazon.com/detective/)

---

## 11. AWS Well-Architected Framework

Apply Generative AI Lens security pillar.

**Pillars:**
1. **Operational Excellence**: Monitoring, incident response
2. **Security**: Identity, detection, protection, data security
3. **Reliability**: Fault tolerance, recovery
4. **Performance Efficiency**: Resource optimization
5. **Cost Optimization**: Cost-effective security controls
6. **Sustainability**: Efficient resource usage

**Tài liệu:**
- [Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [Generative AI Lens](https://docs.aws.amazon.com/wellarchitected/latest/generative-ai-lens/)

---

## 12. Deployment Architecture

### Reference Architecture

```
Internet
   ↓
CloudFront + AWS WAF
   ↓
Application Load Balancer
   ↓
Amazon Cognito (OAuth 2.0)
   ↓
VPC (Private Subnets - Multi-AZ)
   ↓
Amazon ECS (Fargate)
   ├─ MCP Server Containers
   ├─ Task IAM Roles
   └─ Security Groups
   ↓
AWS Secrets Manager
   ↓
Amazon Bedrock
   ├─ AgentCore Identity
   ├─ Guardrails
   └─ Agents
   ↓
Data Sources (S3, RDS, APIs)

Monitoring:
CloudTrail | CloudWatch | GuardDuty | EventBridge
```

**Tài liệu:** [AWS MCP Deployment Guidance](https://aws.amazon.com/solutions/guidance/deploying-model-context-protocol-servers-on-aws/)

---

## 13. Quick Start Checklist

### Initial Setup

- [ ] Enable CloudTrail (all regions)
- [ ] Configure VPC với private subnets
- [ ] Create VPC endpoints cho Bedrock
- [ ] Set up Secrets Manager
- [ ] Configure KMS keys
- [ ] Enable GuardDuty
- [ ] Create IAM roles (least privilege)
- [ ] Deploy security groups
- [ ] Configure CloudWatch alarms

### Bedrock Agent Configuration

- [ ] Create agent với guardrails
- [ ] Configure AgentCore Identity
- [ ] Set up OAuth 2.0
- [ ] Enable return of control
- [ ] Configure user confirmation
- [ ] Test prompt injection protection
- [ ] Enable invocation logging
- [ ] Set up knowledge base (if needed)

### MCP Server Deployment

- [ ] Use CloudFormation/IaC
- [ ] Deploy trong private subnet
- [ ] Assign task IAM role
- [ ] Configure security group
- [ ] Enable container insights
- [ ] Set up log aggregation
- [ ] Configure health checks

### Ongoing Operations

- [ ] Weekly security reviews
- [ ] Monthly access audits
- [ ] Quarterly penetration testing
- [ ] Continuous compliance monitoring
- [ ] Incident response drills
- [ ] Security training
- [ ] Patch management
- [ ] Credential rotation

---

## Tài Liệu Tham Khảo

### AWS Services
- [Amazon Bedrock](https://docs.aws.amazon.com/bedrock/)
- [Bedrock Agents](https://docs.aws.amazon.com/bedrock/latest/userguide/agents.html)
- [Bedrock Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html)
- [AWS IAM](https://docs.aws.amazon.com/IAM/)
- [AWS KMS](https://docs.aws.amazon.com/kms/)
- [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/)
- [Amazon VPC](https://docs.aws.amazon.com/vpc/)
- [AWS PrivateLink](https://docs.aws.amazon.com/vpc/latest/privatelink/)
- [AWS CloudTrail](https://docs.aws.amazon.com/cloudtrail/)
- [Amazon CloudWatch](https://docs.aws.amazon.com/cloudwatch/)
- [Amazon GuardDuty](https://docs.aws.amazon.com/guardduty/)
- [Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/)
- [AWS WAF](https://docs.aws.amazon.com/waf/)
- [Amazon CloudFront](https://docs.aws.amazon.com/cloudfront/)
- [AWS CloudFormation](https://docs.aws.amazon.com/cloudformation/)
- [AWS Config](https://docs.aws.amazon.com/config/)
- [Amazon Detective](https://docs.aws.amazon.com/detective/)

### MCP Resources
- [Model Context Protocol](https://modelcontextprotocol.io/)
- [MCP Specification](https://modelcontextprotocol.io/specification/2025-11-25)
- [MCP Security Best Practices](https://modelcontextprotocol.io/specification/2025-11-25/basic/security_best_practices)
- [AWS MCP Servers](https://github.com/awslabs/mcp)

### AWS Blogs & Guidance
- [AWS MCP Deployment Guidance](https://aws.amazon.com/solutions/guidance/deploying-model-context-protocol-servers-on-aws/)
- [AWS Blog: MCP Authentication](https://aws.amazon.com/blogs/opensource/open-protocols-for-agent-interoperability-part-2-authentication-on-mcp/)
- [Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [Generative AI Lens](https://docs.aws.amazon.com/wellarchitected/latest/generative-ai-lens/)

### Security Standards
- [OAuth 2.0 (RFC 6749)](https://datatracker.ietf.org/doc/html/rfc6749)
- [OAuth 2.0 Security (RFC 9700)](https://datatracker.ietf.org/doc/html/rfc9700)
- [RFC 9728 - OAuth Protected Resource Metadata](https://datatracker.ietf.org/doc/html/rfc9728)
- [RFC 8414 - Authorization Server Metadata](https://datatracker.ietf.org/doc/html/rfc8414)
- [RFC 7523 - JWT Assertions](https://datatracker.ietf.org/doc/html/rfc7523)

---

**Version:** 1.0  
**Last Updated:** 2026-01-24  
**Focus:** AWS MCP Security Implementation
