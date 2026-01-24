# AWS SSO Configuration - Ready to Use

## ✅ Your SSO Details

- **SSO Start URL**: `https://d-9667754ecf.awsapps.com/start`
- **SSO Region**: `us-east-1`
- **Profile Name**: `default`

## 🚀 Quick Setup (Run in your terminal)

```bash
aws configure sso
```

**Enter these values when prompted:**

```
SSO session name: my-sso
SSO start URL: https://d-9667754ecf.awsapps.com/start
SSO region: us-east-1
SSO registration scopes: [press Enter]
```

Browser sẽ mở → **Login to AWS SSO** → Chọn account và role

**Then select:**
```
CLI default region: us-east-1
CLI output format: json
Profile name: default
```

## ✅ Test Connection

```bash
aws sts get-caller-identity --profile default
```

Expected output:
```json
{
  "UserId": "AROAXXXXXXXXX:user@example.com",
  "Account": "123456789012",
  "Arn": "arn:aws:sts::123456789012:assumed-role/RoleName/user@example.com"
}
```

## 🔧 Use with IAM MCP Server

### Option 1: Environment Variable
```bash
export AWS_PROFILE=default
awslabs.iam-mcp-server --readonly
```

### Option 2: Kiro Config

Create `~/.kiro/settings/mcp.json`:
```json
{
  "mcpServers": {
    "awslabs.iam-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.iam-mcp-server@latest", "--readonly"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1"
      }
    }
  }
}
```

### Option 3: Cursor Config

Create `.cursor/mcp.json`:
```json
{
  "mcpServers": {
    "awslabs.iam-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.iam-mcp-server@latest", "--readonly"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1"
      }
    }
  }
}
```

## 📝 Expected ~/.aws/config

After setup, your config will look like:

```ini
[profile default]
sso_session = my-sso
sso_account_id = YOUR_ACCOUNT_ID
sso_role_name = YOUR_ROLE_NAME
region = us-east-1
output = json

[sso-session my-sso]
sso_start_url = https://d-9667754ecf.awsapps.com/start
sso_region = us-east-1
sso_registration_scopes = sso:account:access
```

## 🔄 Daily Usage

### Login (token expires after 8 hours)
```bash
aws sso login --profile default
```

### Check current identity
```bash
aws sts get-caller-identity --profile default
```

### Test IAM access
```bash
aws iam list-users --profile default --max-items 5
```

### Run MCP Server
```bash
export AWS_PROFILE=default
awslabs.iam-mcp-server --readonly
```

## 🎯 Try These Commands

```bash
# List IAM users
aws iam list-users --profile default

# List IAM roles
aws iam list-roles --profile default --max-items 10

# Get your user info
aws iam get-user --profile default

# List policies
aws iam list-policies --profile default --scope Local --max-items 10
```

## ⚠️ Troubleshooting

### Token Expired
```bash
aws sso login --profile default
```

### Browser Doesn't Open
```bash
aws sso login --profile default --no-browser
# Copy URL and paste in browser manually
```

### Check Profile
```bash
aws configure list-profiles
cat ~/.aws/config
```

### Permission Issues
Verify your SSO role has IAM permissions:
- `iam:List*`
- `iam:Get*`
- `iam:Create*` (if not using --readonly)
- `iam:Delete*` (if not using --readonly)

## 📚 Resources

- **SSO Setup Script**: `/home/ubuntu/aws/sso-login-instructions.sh`
- **Full SSO Guide**: `/home/ubuntu/aws/aws-sso-setup.md`
- **IAM MCP Server Blog**: `/home/ubuntu/aws/blog-iam-mcp-server.md`
- **Test Results**: `/home/ubuntu/aws/iam-mcp-server-test-results.md`

---

**Ready to configure!** Run: `aws configure sso` 🚀
