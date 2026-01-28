# Moltbot on AWS with Bedrock

> Deploy [Moltbot](https://github.com/moltbot/moltbot) (formerly Clawdbot) on AWS using Amazon Bedrock and native AWS services. Enterprise-ready, secure, one-click deployment.

English | [简体中文](README_CN.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![AWS](https://img.shields.io/badge/AWS-Bedrock-orange.svg)](https://aws.amazon.com/bedrock/)
[![CloudFormation](https://img.shields.io/badge/IaC-CloudFormation-blue.svg)](https://aws.amazon.com/cloudformation/)

## What is This?

[Moltbot](https://github.com/moltbot/moltbot) (formerly Clawdbot) is an open-source personal AI assistant that connects to WhatsApp, Slack, Discord, and more. This project provides an **AWS-native deployment** using Amazon Bedrock instead of Anthropic/AWS Nova/DeepSeek API keys.

## Why AWS Native?

| Original Moltbot | This Project |
|-------------------|--------------|
| Anthropic API Key | **Amazon Bedrock + IAM** |
| Single model | **Multiple models (Claude, Nova, DeepSeek, etc.)** |
| Tailscale VPN | **SSM Session Manager** |
| Manual setup | **CloudFormation (1-click)** |
| No audit logs | **CloudTrail (automatic)** |
| Public internet | **VPC Endpoints (private)** |

## Key Benefits

- 🔐 **No API Key Management** - IAM roles handle authentication automatically
- 🤖 **Multi-Model Support** - Easily Switch between Claude, Nova, DeepSeek
- 🏢 **Enterprise-Ready** - Full CloudTrail audit logs and compliance support
- 🚀 **One-Click Deploy** - CloudFormation automates everything
- 🔒 **Secure Access** - SSM Session Manager, no public ports exposed
- 💰 **Cost Visibility** - Native AWS cost tracking and optimization

## Quick Start

### ⚡ One-Click Deploy (Recommended - 8 minutes to ready!)

> **Why CloudFormation?** Fully automated setup - no manual configuration needed. Just click, wait 8 minutes, and get your ready-to-use URL!

**Just 3 steps**:
1. ✅ Click "Launch Stack" button below
2. ✅ Select your EC2 key pair in the form
3. ✅ Wait ~8 minutes → Check "Outputs" tab → Copy URL → Start using!

**What happens automatically**:
- Creates VPC, subnets, security groups
- Launches EC2 instance
- Installs Node.js, Docker, Clawdbot
- Configures Bedrock integration
- Generates secure gateway token
- Outputs ready-to-use URL with token

Click to deploy:

| Region | Launch Stack |
|--------|--------------|
| **US West (Oregon)** | [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/review?stackName=clawdbot-bedrock&templateURL=https://sharefile-jiade.s3.cn-northwest-1.amazonaws.com.cn/clawdbot-bedrock.yaml) |
| **US East (N. Virginia)** | [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=clawdbot-bedrock&templateURL=https://sharefile-jiade.s3.cn-northwest-1.amazonaws.com.cn/clawdbot-bedrock.yaml) |
| **EU (Ireland)** | [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/review?stackName=clawdbot-bedrock&templateURL=https://sharefile-jiade.s3.cn-northwest-1.amazonaws.com.cn/clawdbot-bedrock.yaml) |
| **Asia Pacific (Tokyo)** | [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-northeast-1#/stacks/create/review?stackName=clawdbot-bedrock&templateURL=https://sharefile-jiade.s3.cn-northwest-1.amazonaws.com.cn/clawdbot-bedrock.yaml) |

> **Note**: Using Global CRIS profiles - works in 30+ regions worldwide. Deploy in any region, requests auto-route to optimal locations.

**After deployment (~8 minutes), check CloudFormation Outputs tab**:

1. **Install SSM Plugin**: Click link in `Step1InstallSSMPlugin` (one-time setup)
2. **Port Forwarding**: Copy command from `Step2PortForwarding`, run on your computer (keep terminal open)
3. **Open URL**: Copy URL from `Step3AccessURL`, open in browser (token included!)
4. **Start Chatting**: Connect WhatsApp/Telegram/Discord in Web UI


![CloudFormation Outputs](images/20260128-105244.jpeg)
![Clawdbot Web UI](images/20260128-105059.jpg)

> **Before deploying**:
> - Before deploying, enable Bedrock models in Bedrock Console
> - Create an EC2 key pair in your target region
> - Lambda will automatically validate Bedrock access during deployment

### Alternative: Download and Upload

1. Download: [clawdbot-bedrock.yaml](clawdbot-bedrock.yaml)
2. Go to [CloudFormation Console](https://console.aws.amazon.com/cloudformation/)
3. Upload template and deploy

| Region | Launch Stack |
|--------|--------------|
| **US East (N. Virginia)** | [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=clawdbot-bedrock&templateURL=https://clawdbot-templates-jiade.s3.amazonaws.com/clawdbot-bedrock.yaml) |
| **US West (Oregon)** | [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/review?stackName=clawdbot-bedrock&templateURL=https://clawdbot-templates-jiade.s3.amazonaws.com/clawdbot-bedrock.yaml) |

> **Before deploying**: 
> 1. Enable Bedrock models in [Bedrock Console](https://console.aws.amazon.com/bedrock/)
> 2. Create an EC2 key pair in your target region

### Alternative: CLI Deploy

- AWS account with Bedrock access
- [AWS CLI](https://aws.amazon.com/cli/) installed
- [SSM Session Manager Plugin](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html) installed
- EC2 key pair created

### Manual Deploy (Alternative)

**Using helper script**:

```bash
./scripts/deploy.sh clawdbot-bedrock us-west-2 your-keypair
```

**Using AWS CLI**:

```bash
aws cloudformation create-stack \
  --stack-name clawdbot-bedrock \
  --template-body file://cloudformation/clawdbot-bedrock.yaml \
  --parameters ParameterKey=KeyPairName,ParameterValue=your-keypair \
  --capabilities CAPABILITY_IAM \
  --region us-west-2

# Wait for completion
aws cloudformation wait stack-create-complete \
  --stack-name clawdbot-bedrock \
  --region us-west-2
```

> **Note**: Lambda pre-check runs automatically during deployment. If it fails, check CloudFormation events for details.

### Access Clawdbot

```bash
# Get instance ID
INSTANCE_ID=$(aws cloudformation describe-stacks \
  --stack-name clawdbot-bedrock \
  --query 'Stacks[0].Outputs[?OutputKey==`InstanceId`].OutputValue' \
  --output text)

# Start port forwarding
aws ssm start-session \
  --target $INSTANCE_ID \
  --document-name AWS-StartPortForwardingSession \
  --parameters '{"portNumber":["18789"],"localPortNumber":["18789"]}'

# Get token (new terminal)
aws ssm start-session --target $INSTANCE_ID
sudo su - ubuntu
cat ~/.clawdbot/gateway_token.txt

# Open browser
http://localhost:18789/?token=<your-token>
```

## How to Use Clawdbot

### Connect Messaging Platforms

#### WhatsApp (Recommended)

1. **In Web UI**: Click "Channels" → "Add Channel" → "WhatsApp"
2. **Scan QR Code**: Use WhatsApp on your phone
   - Open WhatsApp → Settings → Linked Devices
   - Tap "Link a Device"
   - Scan the QR code displayed
3. **Verify**: Send a test message to your Clawdbot number

**Tip**: Use a dedicated phone number or enable `selfChatMode` for personal number.

#### Telegram

1. **Create Bot**: Message [@BotFather](https://t.me/botfather)
   ```
   /newbot
   Choose a name: My Clawdbot
   Choose a username: my_clawdbot_bot
   ```
2. **Copy Token**: BotFather will give you a token like `123456:ABC-DEF...`
3. **Configure**: In Web UI, add Telegram channel with your bot token
4. **Test**: Send `/start` to your bot on Telegram

#### Discord

1. **Create Bot**: Visit [Discord Developer Portal](https://discord.com/developers/applications)
   - Click "New Application"
   - Go to "Bot" → "Add Bot"
   - Copy bot token
   - Enable intents: Message Content, Server Members
2. **Invite Bot**: Generate invite URL with permissions
   ```
   https://discord.com/api/oauth2/authorize?client_id=YOUR_CLIENT_ID&permissions=8&scope=bot
   ```
3. **Configure**: In Web UI, add Discord channel with bot token
4. **Test**: Mention your bot in a Discord channel

#### Slack

1. **Create App**: Visit [Slack API](https://api.slack.com/apps)
2. **Configure Bot**: Add bot token scopes (chat:write, channels:history)
3. **Install**: Install app to your workspace
4. **Configure**: In Web UI, add Slack channel
5. **Test**: Invite bot to a channel and mention it



### Using Clawdbot

#### Send Messages

**Via WhatsApp/Telegram/Discord**: Just send a message!

```
You: What's the weather today?
Clawdbot: Let me check that for you...
```

**Via CLI**:
```bash
# SSH/SSM to instance
clawdbot message send --to +1234567890 --message "Hello"
```

#### Chat Commands

Send these in any connected channel:

| Command | Description |
|---------|-------------|
| `/status` | Show session status (model, tokens, cost) |
| `/new` or `/reset` | Start a new conversation |
| `/think high` | Enable deep thinking mode |
| `/help` | Show available commands |

#### Voice Messages

**WhatsApp/Telegram**: Send voice notes directly - Clawdbot will transcribe and respond!

#### Browser Control

```
You: Open google.com and search for "AWS Bedrock"
Clawdbot: *Opens browser, performs search, returns results*
```

#### Scheduled Tasks

```
You: Remind me every day at 9am to check emails
Clawdbot: *Creates cron job*
```

### Advanced Features

#### Skills

```bash
# List available skills
clawdbot skills list

# Install a skill
clawdbot skills install voice-generation

# View installed skills
clawdbot skills installed
```

#### Custom Prompts

Create `~/clawd/system.md` on the instance:

```markdown
You are my personal assistant. Be concise and helpful.
Always respond in a friendly tone.
```

#### Multi-Agent Routing

Configure different agents for different channels in Web UI.

For detailed guides, visit [Moltbot Documentation](https://docs.molt.bot/).

## Architecture

```
Your Phone/Computer → WhatsApp/Telegram → EC2 (Moltbot) → Bedrock (Claude)
                                              ↓
                                         Your Data Stays Here
                                         (Secure, Private, Audited)
```

### Why EC2 + Bedrock?

**🔒 Security**: All data in your AWS account, IAM authentication, no API keys to leak

**💰 Cost-Effective**: ~$70-115/month total, cheaper than multiple ChatGPT subscriptions for teams

**🛡️ Reliable**: 24/7 availability, auto-restart, CloudWatch monitoring

**📊 Transparent**: CloudTrail logs every API call, Cost Explorer tracks spending

**🌐 Multi-Platform**: One instance serves WhatsApp, Telegram, Discord, Slack simultaneously

### What Runs on EC2

- **Moltbot Gateway**: Routes messages, manages sessions (~300MB RAM)
- **Browser Control**: Web automation when needed (~500MB RAM)
- **Docker Sandbox**: Isolated code execution (secure)
- **Total Usage**: ~1-2GB of 30GB disk, ~500MB-1GB RAM

### Data Flow Example

```
You: "Summarize this document"
  ↓ WhatsApp (public internet)
EC2: Receives message, authenticates with IAM
  ↓ VPC Endpoint (private network)
Bedrock: Claude processes document
  ↓ VPC Endpoint (private network)
EC2: Formats response
  ↓ WhatsApp (public internet)
You: Receives summary

Cost: ~$0.01 per request
Time: 2-5 seconds
Security: All AWS traffic via private network
```

**Key Components**:
- **EC2 Instance**: Runs Clawdbot gateway and browser control
- **IAM Role**: Authenticates with Bedrock (no API keys)
- **SSM Session Manager**: Secure access without public ports
- **VPC Endpoints**: Private network access to Bedrock
- **Lambda Pre-Check**: Validates environment before deployment

## Cost Breakdown

### Monthly Infrastructure Cost

| Service | Configuration | Monthly Cost |
|---------|--------------|--------------|
| EC2 (t3.medium) | 2 vCPU, 4GB RAM | $30.37 |
| EBS (gp3) | 30GB | $2.40 |
| VPC Endpoints | 3 endpoints | $21.60 |
| Data Transfer | VPC endpoint processing | $5-10 |
| **Subtotal** | | **$60-65** |

### Bedrock Usage Cost

| Model | Input | Output |
|-------|-------|--------|
| Claude Sonnet 4.5 | $3/1M tokens | $15/1M tokens |
| Claude 3.5 Sonnet v2 | $3/1M tokens | $15/1M tokens |
| Claude 3.5 Haiku | $1/1M tokens | $5/1M tokens |
| Claude 3 Haiku | $0.25/1M tokens | $1.25/1M tokens |

**Example**: 100 conversations/day with Sonnet 4.5 ≈ $10-15/month

**Total**: ~$70-80/month for light usage

### Cost Optimization

- Use Haiku instead of Sonnet: 75% cheaper
- Disable VPC endpoints: Save $22/month (less secure)
- Use Savings Plans: Save 30-40% on EC2
- Use Spot Instances: Save 70% on EC2 (may be interrupted)

## Configuration

### Supported Models

```yaml
# In CloudFormation parameters
ClawdbotModel:
  - anthropic.claude-sonnet-4-5-20250929-v1:0  # Default, most capable
  - anthropic.claude-3-5-sonnet-20241022-v2:0  # Stable alternative
  - anthropic.claude-3-5-haiku-20241022-v1:0   # Faster, cheaper
  - anthropic.claude-3-haiku-20240307-v1:0     # Fastest/cheapest
```

**Model Selection Guide**:
- **Claude Sonnet 4.5** (default): Best performance, coding, and complex reasoning. Available in 30+ regions worldwide.
- **Claude 3.5 Sonnet v2**: Excellent balance of performance and availability.
- **Claude 3.5 Haiku**: Fast and cost-effective for simpler tasks.

### Instance Types

```yaml
InstanceType:
  - t3.small   # Light usage, ~$15/month
  - t3.medium  # Recommended, ~$30/month
  - t3.large   # Heavy usage, ~$60/month
```

### VPC Endpoints

```yaml
CreateVPCEndpoints: true   # Recommended for production
  # Pros: Private network, more secure, lower latency
  # Cons: +$22/month

CreateVPCEndpoints: false  # For cost optimization
  # Pros: Save $22/month
  # Cons: Traffic goes through public internet
```

## Security Features

### 1. IAM Role Authentication

No API keys to manage. The EC2 instance uses an IAM role to authenticate with Bedrock:

```json
{
  "Effect": "Allow",
  "Action": [
    "bedrock:InvokeModel",
    "bedrock:InvokeModelWithResponseStream"
  ],
  "Resource": "*"
}
```

### 2. SSM Session Manager

No SSH keys needed. Access via AWS Systems Manager:

- ✅ No public ports (except optional SSH fallback)
- ✅ Automatic session logging
- ✅ CloudTrail audit trail
- ✅ Session timeout controls

### 3. VPC Endpoints

Traffic stays within AWS network:

- ✅ Bedrock API calls don't go through internet
- ✅ Lower latency
- ✅ Compliance-friendly

### 4. Docker Sandbox

Non-main sessions run in isolated Docker containers:

```json
{
  "sandbox": {
    "mode": "non-main",
    "allowlist": ["bash", "read", "write"],
    "denylist": ["browser", "gateway"]
  }
}
```

## Monitoring & Audit

### CloudTrail Logs

All Bedrock API calls are automatically logged:

```bash
aws cloudtrail lookup-events \
  --lookup-attributes AttributeKey=EventName,AttributeValue=InvokeModel \
  --region us-west-2
```

### CloudWatch Logs

```bash
# View setup logs
aws logs tail /var/log/clawdbot-setup.log --follow

# View SSM session logs
aws logs tail /aws/ssm/session-logs --follow
```

### Cost Monitoring

```bash
# View Bedrock costs
aws ce get-cost-and-usage \
  --time-period Start=2026-01-01,End=2026-01-31 \
  --granularity DAILY \
  --metrics BlendedCost \
  --filter '{"Dimensions":{"Key":"SERVICE","Values":["Amazon Bedrock"]}}'
```

## Troubleshooting

### Pre-Check Failed

```bash
# View Lambda logs
aws logs tail /aws/lambda/clawdbot-bedrock-bedrock-precheck --follow

# Common issues:
# 1. Model not enabled → Enable in Bedrock Console
# 2. Region not supported → Use us-east-1 or us-west-2
# 3. Permission denied → Check IAM permissions
```

### Cannot Connect via SSM

```bash
# Check SSM agent status
aws ssm describe-instance-information \
  --filters "Key=InstanceIds,Values=$INSTANCE_ID"

# Check IAM role
aws ec2 describe-instances \
  --instance-ids $INSTANCE_ID \
  --query 'Reservations[0].Instances[0].IamInstanceProfile'
```

### Bedrock API Errors

```bash
# Test Bedrock access
aws bedrock-runtime invoke-model \
  --model-id anthropic.claude-3-5-sonnet-20241022-v2:0 \
  --body '{"anthropic_version":"bedrock-2023-05-31","max_tokens":10,"messages":[{"role":"user","content":"Hi"}]}' \
  --region us-west-2 \
  output.txt

# View Clawdbot logs
journalctl --user -u clawdbot-gateway -n 100
```

## Project Structure

```
clawdbot-aws-bedrock/
├── README.md                          # This file
├── cloudformation/
│   └── clawdbot-bedrock.yaml         # Main CloudFormation template
├── scripts/
│   ├── bedrock-precheck.sh           # Pre-deployment check script
│   └── deploy.sh                     # Deployment helper script
├── lambda/
│   └── precheck/
│       ├── index.py                  # Lambda pre-check function
│       └── requirements.txt          # Python dependencies
└── docs/
    ├── DEPLOYMENT.md                 # Detailed deployment guide
    ├── SECURITY.md                   # Security best practices
    └── TROUBLESHOOTING.md            # Common issues and solutions
```

## Comparison with Original Clawdbot

### Original (Anthropic API + Tailscale)

```bash
# Requires:
- Anthropic API key (manual management)
- Tailscale account (third-party service)
- Manual security configuration

# Cost: ~$40/month + API fees
```

### This Project (Bedrock + SSM)

```bash
# Requires:
- AWS account only
- IAM authentication (automatic)
- Native AWS services

# Cost: ~$65/month + Bedrock fees
# Extra $25/month buys you:
# - No API key management
# - Multi-model support (Claude, Nova, DeepSeek)
# - Full audit logs
# - Enterprise compliance
# - AWS support coverage
```

## Contributing

Contributions welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Submit a pull request

## License

This deployment template is provided as-is. Clawdbot itself is licensed under its original license.

## Resources

- [Moltbot Official Docs](https://docs.molt.bot/)
- [Moltbot GitHub](https://github.com/moltbot/moltbot)
- [Amazon Bedrock Docs](https://docs.aws.amazon.com/bedrock/)
- [SSM Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html)

## Support

- **Moltbot Issues**: [GitHub Issues](https://github.com/moltbot/moltbot/issues)
- **AWS Bedrock**: [AWS re:Post](https://repost.aws/tags/bedrock)
- **This Project**: [GitHub Issues](https://github.com/your-repo/clawdbot-aws-bedrock/issues)

---

**Built by builder + Kiro for AWS customers and partners**

Deploy your personal AI assistant on AWS infrastructure you control.
