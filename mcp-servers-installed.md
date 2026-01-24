# ✅ AWS MCP Servers - Setup Complete!

## 🎉 Installed Servers (6 servers)

### 1. ✅ IAM MCP Server
**Purpose**: Quản lý IAM users, roles, groups, policies  
**Mode**: Read-only (safe)  
**Use cases**:
- List IAM users/roles/groups
- Get user details và permissions
- Test permissions với simulation
- Audit IAM configurations

### 2. ✅ AWS Documentation MCP Server
**Purpose**: Access latest AWS documentation  
**Use cases**:
- Get up-to-date AWS service docs
- API references
- Best practices
- Code examples

### 3. ✅ AWS CDK MCP Server
**Purpose**: AWS CDK development với security compliance  
**Use cases**:
- Generate CDK code
- Infrastructure as Code
- Security scanning
- Best practices compliance

### 4. ✅ AWS Serverless MCP Server
**Purpose**: Serverless application lifecycle  
**Mode**: Read-only (safe)  
**Use cases**:
- Lambda function management
- API Gateway configuration
- EventBridge rules
- Step Functions workflows
- DynamoDB operations

### 5. ✅ Cost Explorer MCP Server
**Purpose**: Cost analysis và reporting  
**Use cases**:
- Analyze AWS costs
- Cost forecasting
- Budget tracking
- Cost optimization recommendations

### 6. ✅ CloudWatch MCP Server
**Purpose**: Metrics, alarms, logs analysis  
**Use cases**:
- Monitor application performance
- Analyze logs
- Set up alarms
- Troubleshoot issues

## 📁 Configuration Files

### Kiro Config
**Location**: `~/.kiro/settings/mcp.json`  
**Status**: ✅ Created

### Cursor Config
**Location**: `.cursor/mcp.json`  
**Status**: Ready to copy

```json
{
  "mcpServers": {
    "awslabs.iam-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.iam-mcp-server@latest", "--readonly"],
      "env": {"AWS_REGION": "us-east-1"}
    },
    "awslabs.aws-documentation-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.aws-documentation-mcp-server@latest"],
      "env": {"AWS_REGION": "us-east-1"}
    },
    "awslabs.cdk-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.cdk-mcp-server@latest"],
      "env": {"AWS_REGION": "us-east-1"}
    },
    "awslabs.aws-serverless-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.aws-serverless-mcp-server@latest", "--readonly"],
      "env": {"AWS_REGION": "us-east-1"}
    },
    "awslabs.cost-explorer-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.cost-explorer-mcp-server@latest"],
      "env": {"AWS_REGION": "us-east-1"}
    },
    "awslabs.cloudwatch-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.cloudwatch-mcp-server@latest"],
      "env": {"AWS_REGION": "us-east-1"}
    }
  }
}
```

## 🎯 Example Prompts

### IAM Management
```
"List all IAM users in my account"
"Show me the policies attached to user admin"
"Test if user X can access S3 bucket Y"
```

### Documentation
```
"Show me the latest Lambda documentation"
"What are the best practices for DynamoDB?"
"Get API reference for S3 PutObject"
```

### CDK Development
```
"Generate CDK code for a Lambda function with API Gateway"
"Create a CDK stack for a serverless application"
"Show me CDK best practices for security"
```

### Serverless
```
"List all Lambda functions"
"Show me API Gateway endpoints"
"Get EventBridge rules"
```

### Cost Analysis
```
"What are my AWS costs for last month?"
"Show me cost breakdown by service"
"Forecast my costs for next month"
```

### Monitoring
```
"Show me CloudWatch alarms"
"Get logs for Lambda function X"
"What are the metrics for my application?"
```

## 📊 All 64 Available AWS MCP Servers

### 📚 Documentation (2)
- AWS Documentation MCP Server ✅
- AWS Knowledge MCP Server

### 🏗️ Infrastructure & Deployment (12)
- AWS Cloud Control API
- AWS CloudFormation
- AWS CDK ✅
- AWS Terraform
- Amazon EKS
- Amazon ECS
- Finch
- AWS Serverless ✅
- AWS Lambda Tool
- AWS Support

### 🤖 AI & Machine Learning (10)
- Bedrock Knowledge Bases Retrieval
- Amazon Kendra Index
- Amazon Q Business
- Amazon Q Index
- Document Loader
- Amazon Nova Canvas
- Bedrock Data Automation
- Bedrock Custom Model Import
- SageMaker AI
- Bedrock AgentCore

### 📊 Data & Analytics (18)
- DynamoDB
- Aurora PostgreSQL
- S3 Tables
- Aurora MySQL
- Aurora DSQL
- DocumentDB
- Neptune
- Keyspaces
- Timestream for InfluxDB
- ElastiCache
- ElastiCache/MemoryDB for Valkey
- ElastiCache for Memcached
- AppSync
- IoT Sitewise
- Data Processing
- Redshift
- SageMaker Unified Studio (Spark)

### 🛠️ Developer Tools (7)
- Git Repo Research
- Code Documentation Generation
- AWS Diagram
- Frontend
- Synthetic Data
- IAM ✅
- AWS MSK

### 📡 Integration & Messaging (5)
- OpenAPI
- SNS/SQS
- Amazon MQ
- Step Functions
- Location Service

### 💰 Cost & Operations (10)
- Billing and Cost Management
- AWS Pricing
- Cost Explorer ✅
- Managed Prometheus
- CloudWatch Application Signals
- CloudWatch ✅
- CloudTrail
- Well-Architected Security Assessment

### 🧬 Healthcare & Lifesciences (2)
- AWS HealthOmics
- HealthLake

## 🚀 Install More Servers

```bash
# Data & Analytics
uv tool install awslabs.dynamodb-mcp-server
uv tool install awslabs.s3-tables-mcp-server

# Infrastructure
uv tool install awslabs.terraform-mcp-server
uv tool install awslabs.eks-mcp-server

# AI & ML
uv tool install awslabs.bedrock-kb-retrieval-mcp-server
uv tool install awslabs.nova-canvas-mcp-server

# Messaging
uv tool install awslabs.sns-sqs-mcp-server
uv tool install awslabs.stepfunctions-tool-mcp-server
```

## 📚 Resources

- **AWS MCP Docs**: https://awslabs.github.io/mcp/
- **GitHub**: https://github.com/awslabs/mcp
- **Blog**: https://aws.amazon.com/blogs/machine-learning/introducing-aws-mcp-servers-for-code-assistants-part-1/

---

**Setup Date**: 2026-01-24  
**Status**: ✅ 6 SERVERS READY  
**AWS Account**: 067847735476  
**Region**: us-east-1
