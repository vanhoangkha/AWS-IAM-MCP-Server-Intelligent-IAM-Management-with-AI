# AWS IAM MCP Server - Kết Quả Cài Đặt và Test

## ✅ Cài Đặt Thành Công

### 1. Cài đặt uv package manager
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```
- ✅ Version: uv 0.9.26
- ✅ Location: /home/ubuntu/.local/bin

### 2. Cài đặt AWS IAM MCP Server
```bash
uv tool install awslabs.iam-mcp-server
```
- ✅ Package: awslabs.iam-mcp-server v1.0.12
- ✅ Dependencies: 44 packages installed
- ✅ Executable: awslabs.iam-mcp-server

## 📋 Thông Tin Server

### Available Tools (19 tools)

**User Management:**
1. `list_users` - List IAM users với filtering
2. `get_user` - Get chi tiết user (policies, groups, keys)
3. `create_user` - Create user mới
4. `delete_user` - Delete user (với force cleanup)

**Role Management:**
5. `list_roles` - List IAM roles
6. `create_role` - Create role với trust policy

**Group Management:**
7. `list_groups` - List IAM groups
8. `get_group` - Get group details (members, policies)
9. `create_group` - Create group mới
10. `delete_group` - Delete group (với force cleanup)

**Policy Management:**
11. `list_policies` - List managed policies
12. `attach_user_policy` - Attach policy to user
13. `detach_user_policy` - Detach policy from user

**Access Key Management:**
14. `create_access_key` - Create access key
15. `delete_access_key` - Delete access key

**Security Analysis:**
16. `simulate_principal_policy` - Test permissions

**Inline Policy Management:**
17. `put_user_policy` - Create/update inline policy
18. `get_user_policy` - Get inline policy
19. `delete_user_policy` - Delete inline policy

### Security Features

- ✅ **Read-only mode** (`--readonly` flag)
- ✅ **Force delete** với automatic cleanup
- ✅ **Policy simulation** - test trước khi apply
- ✅ **Permissions boundary** support

## 🔧 Configuration

### MCP Client Configuration (Kiro)

Tạo file `~/.kiro/settings/mcp.json`:

```json
{
  "mcpServers": {
    "awslabs.iam-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.iam-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "your-aws-profile",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    }
  }
}
```

### Read-Only Mode Configuration

```json
{
  "mcpServers": {
    "awslabs.iam-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.iam-mcp-server@latest", "--readonly"],
      "env": {
        "AWS_PROFILE": "your-aws-profile",
        "AWS_REGION": "us-east-1"
      }
    }
  }
}
```

## 🎯 Use Cases

### 1. List Users
```
You: "List all IAM users in the account"
AI: Lists users with details
```

### 2. Get User Details
```
You: "Show me details for user john.doe including policies and groups"
AI: Returns comprehensive user information
```

### 3. Test Permissions
```
You: "Test if user developer can read from S3 bucket my-app-data"
AI: Simulates policy and returns allowed/denied
```

### 4. Create User (Write Mode)
```
You: "Create a new developer user alice.nguyen in /developers/ path"
AI: Creates user with specified path
```

### 5. Audit IAM
```
You: "Show all users in Developers group with their attached policies"
AI: Lists group members and their permissions
```

## ⚠️ Requirements

### AWS Credentials
Cần configure AWS credentials bằng 1 trong 3 cách:

1. **AWS Profile** (recommended):
```bash
export AWS_PROFILE=your-profile-name
```

2. **Environment Variables**:
```bash
export AWS_ACCESS_KEY_ID=your-key
export AWS_SECRET_ACCESS_KEY=your-secret
export AWS_REGION=us-east-1
```

3. **IAM Roles**: Tự động khi chạy trên EC2/Lambda

### IAM Permissions
Server cần ~30 IAM actions:
- `iam:List*` (users, roles, groups, policies)
- `iam:Get*` (user, role, group, policy details)
- `iam:Create*` (users, roles, groups)
- `iam:Delete*` (users, roles, groups)
- `iam:Attach*` / `iam:Detach*` (policies)
- `iam:Put*` / `iam:Delete*` (inline policies)
- `iam:SimulatePrincipalPolicy`

## 🚀 Next Steps

1. **Configure AWS credentials**
2. **Add to MCP client** (Kiro/Cursor/Cline)
3. **Test in read-only mode** first
4. **Enable write mode** when ready

## 📚 Resources

- **Documentation**: https://awslabs.github.io/mcp/servers/iam-mcp-server
- **GitHub**: https://github.com/awslabs/mcp
- **Blog**: /home/ubuntu/aws/blog-iam-mcp-server.md

---

**Status**: ✅ Installation Complete  
**Version**: 1.0.12  
**Date**: 2026-01-24
