# AWS SSO Setup cho IAM MCP Server

## Quick Start

### 1. Configure AWS SSO

```bash
aws configure sso
```

**Nhập thông tin:**
- SSO session name: `my-sso`
- SSO start URL: `https://your-org.awsapps.com/start`
- SSO region: `us-east-1`
- SSO registration scopes: `sso:account:access`

**Chọn account và role:**
- Account: Chọn AWS account
- Role: Chọn IAM role (e.g., AdministratorAccess)
- CLI default region: `us-east-1`
- CLI default output: `json`
- CLI profile name: `my-profile`

### 2. Login SSO

```bash
aws sso login --profile my-profile
```

Browser sẽ mở → Login → Approve

### 3. Test Connection

```bash
aws sts get-caller-identity --profile my-profile
```

Output:
```json
{
  "UserId": "AROAXXXXXXXXX:user@example.com",
  "Account": "123456789012",
  "Arn": "arn:aws:sts::123456789012:assumed-role/RoleName/user@example.com"
}
```

## Configure IAM MCP Server với SSO

### Option 1: Environment Variable

```bash
export AWS_PROFILE=my-profile
awslabs.iam-mcp-server --readonly
```

### Option 2: MCP Client Config

**Kiro** (`~/.kiro/settings/mcp.json`):
```json
{
  "mcpServers": {
    "awslabs.iam-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.iam-mcp-server@latest", "--readonly"],
      "env": {
        "AWS_PROFILE": "my-profile",
        "AWS_REGION": "us-east-1"
      }
    }
  }
}
```

**Cursor** (`.cursor/mcp.json`):
```json
{
  "mcpServers": {
    "awslabs.iam-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.iam-mcp-server@latest", "--readonly"],
      "env": {
        "AWS_PROFILE": "my-profile",
        "AWS_REGION": "us-east-1"
      }
    }
  }
}
```

## AWS SSO Configuration File

File: `~/.aws/config`

```ini
[profile my-profile]
sso_session = my-sso
sso_account_id = 123456789012
sso_role_name = AdministratorAccess
region = us-east-1
output = json

[sso-session my-sso]
sso_start_url = https://your-org.awsapps.com/start
sso_region = us-east-1
sso_registration_scopes = sso:account:access
```

## Common Commands

### Login
```bash
aws sso login --profile my-profile
```

### Logout
```bash
aws sso logout --profile my-profile
```

### Check Session
```bash
aws sts get-caller-identity --profile my-profile
```

### List Profiles
```bash
aws configure list-profiles
```

### Refresh Token
```bash
# Token expires sau 8 hours (default)
aws sso login --profile my-profile
```

## Multiple Profiles

```ini
# ~/.aws/config

[profile dev]
sso_session = my-sso
sso_account_id = 111111111111
sso_role_name = DeveloperAccess
region = us-east-1

[profile prod]
sso_session = my-sso
sso_account_id = 222222222222
sso_role_name = ReadOnlyAccess
region = us-east-1

[sso-session my-sso]
sso_start_url = https://your-org.awsapps.com/start
sso_region = us-east-1
sso_registration_scopes = sso:account:access
```

**Switch profiles:**
```bash
export AWS_PROFILE=dev
# or
export AWS_PROFILE=prod
```

## Troubleshooting

### Token Expired
```bash
# Error: Token has expired
aws sso login --profile my-profile
```

### Profile Not Found
```bash
# Check available profiles
aws configure list-profiles

# Reconfigure
aws configure sso --profile my-profile
```

### Permission Denied
```bash
# Check current identity
aws sts get-caller-identity --profile my-profile

# Verify IAM permissions
aws iam list-users --profile my-profile
```

### Browser Not Opening
```bash
# Manual login
aws sso login --profile my-profile --no-browser

# Copy URL và paste vào browser
```

## IAM Permissions Required

Để sử dụng IAM MCP Server, SSO role cần permissions:

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": [
      "iam:List*",
      "iam:Get*",
      "iam:Create*",
      "iam:Delete*",
      "iam:Attach*",
      "iam:Detach*",
      "iam:Put*",
      "iam:SimulatePrincipalPolicy"
    ],
    "Resource": "*"
  }]
}
```

**Recommended roles:**
- `AdministratorAccess` (full access)
- `IAMFullAccess` (IAM only)
- Custom role với specific permissions

## Best Practices

1. **Use Read-Only Mode** khi explore:
   ```bash
   awslabs.iam-mcp-server --readonly
   ```

2. **Separate Profiles** cho dev/prod:
   ```bash
   export AWS_PROFILE=dev  # for testing
   export AWS_PROFILE=prod # for production
   ```

3. **Regular Token Refresh**:
   - SSO tokens expire sau 8 hours
   - Login lại khi cần

4. **Least Privilege**:
   - Chỉ request permissions cần thiết
   - Use read-only role khi có thể

5. **Audit Logging**:
   - CloudTrail tracks tất cả SSO logins
   - Review access logs regularly

## Quick Test

```bash
# 1. Login
aws sso login --profile my-profile

# 2. Test IAM access
aws iam list-users --profile my-profile --max-items 5

# 3. Run MCP server
export AWS_PROFILE=my-profile
awslabs.iam-mcp-server --readonly
```

## Resources

- [AWS SSO Documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html)
- [AWS CLI SSO Guide](https://docs.aws.amazon.com/cli/latest/userguide/sso-configure-profile-token.html)
- [IAM MCP Server Docs](https://awslabs.github.io/mcp/servers/iam-mcp-server)
