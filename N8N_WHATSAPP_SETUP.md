# n8n WhatsApp Notification Setup

Panduan lengkap untuk setup n8n workflow yang mengirim notifikasi build Vuboi Token ke WhatsApp.

## 📋 Prerequisites

- ✅ n8n Web instance sudah running
- ✅ WhatsApp API credentials (API Key, URL, Target Number)
- ✅ GitHub repository dengan access token (optional untuk advanced setup)
- ✅ Basic understanding of n8n workflows

---

## 🚀 Quick Setup (5 steps)

### Step 1: Setup n8n Environment Variables

Di n8n Admin Panel, set environment variables:

```
VUBOI_WEBHOOK_ID: vuboi-token-build-{{ random }}
WHATSAPP_API_URL: https://your-whatsapp-api.com/api
WHATSAPP_API_KEY: your_whatsapp_api_key_here
WHATSAPP_TARGET_NUMBER: 62812345678 atau 62-group-id
```

### Step 2: Create n8n Workflow

1. Open n8n Dashboard
2. Click **New Workflow**
3. Name: `Vuboi Token - Build to WhatsApp`
4. Import JSON dari `n8n-workflow-github-to-whatsapp.json`

Atau manual setup dengan nodes di bawah.

### Step 3: Add Webhook Node

1. Add **Webhook** node
2. Configure:
   - Method: POST
   - Path: `vuboi-token-build`
   - Authentication: Header Auth
3. Copy webhook URL (akan digunakan di GitHub Actions)

### Step 4: Configure GitHub Actions

Edit `.github/workflows/sync-tokens.yml`:

Tambahkan step sebelum "Workflow Summary":

```yaml
# Notify via n8n Webhook
- name: "📢 Send Build Notification"
  if: always()
  run: |
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "workflow_run": {
          "conclusion": "${{ job.status }}",
          "name": "Sync & Build Design Tokens",
          "head_commit": {
            "message": "${{ github.event.head_commit.message }}"
          },
          "head_branch": "${{ github.ref_name }}",
          "html_url": "${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}",
          "updated_at": "$(date -u +'%Y-%m-%dT%H:%M:%SZ')"
        },
        "repository": {
          "name": "${{ github.repository }}"
        },
        "sender": {
          "login": "${{ github.actor }}"
        }
      }' \
      ${{ secrets.N8N_WEBHOOK_URL }}
```

### Step 5: Test Workflow

1. Deploy workflow di n8n
2. Manual trigger GitHub Actions build
3. Check n8n execution logs
4. Verify WhatsApp message received

---

## 📝 Workflow Nodes Explanation

### 1. **Webhook Trigger**
Menerima POST request dari GitHub Actions dengan build status dan metadata.

**Input:**
```json
{
  "workflow_run": {
    "conclusion": "success|failure",
    "name": "workflow name",
    "head_commit": { "message": "commit message" },
    "head_branch": "main",
    "html_url": "link to run",
    "updated_at": "timestamp"
  },
  "repository": { "name": "repo name" },
  "sender": { "login": "actor" }
}
```

### 2. **Extract GitHub Data**
Parse dan extract data penting dari payload GitHub.

**Output:**
```json
{
  "status": "success",
  "name": "Sync & Build Design Tokens",
  "head_commit": "commit message",
  "branch": "main",
  "url": "workflow URL",
  "timestamp": "ISO timestamp",
  "repository": "repo name",
  "actor": "username"
}
```

### 3. **Format Build Info**
Prepare informasi untuk message formatting.

**Output:**
```json
{
  "success": true,
  "failed": false,
  "status_emoji": "✅",
  "platforms": [...]
}
```

### 4. **Create WhatsApp Message**
Format pesan WhatsApp dengan informasi lengkap.

**Output:**
```
🎨 *Vuboi Design Token Build*

✅ *Status:* SUCCESS ✅

*Repository:* alfariesh/vuboi-token
*Branch:* main
*Commit:* 43d74d9...
*Author:* github-actions[bot]

📦 *Platforms Built:*
• 🌐 Web (CSS, SCSS, JS, TS, JSON)
• 📱 Mobile (Flutter, Compose)
• 🤖 Android (XML)
• 🍎 iOS (Swift)

🔗 *Link:* https://github.com/...
⏱️ *Time:* 10/20/2025, 5:00:00 PM
```

### 5. **Send WhatsApp Message**
HTTP request ke WhatsApp API dengan pesan.

**Request:**
```
POST {{ WHATSAPP_API_URL }}/send
Authorization: Bearer {{ WHATSAPP_API_KEY }}
Content-Type: application/json

{
  "to": "{{ WHATSAPP_TARGET_NUMBER }}",
  "body": "formatted message",
  "type": "text"
}
```

### 6. **Log Result**
Simpan hasil untuk tracking dan debugging.

**Logged Data:**
- Timestamp
- Repository name
- Build status
- Branch
- Commit
- WhatsApp delivery status

### 7. **Response**
Return success response ke GitHub Actions.

---

## 🔧 Customization Options

### Different Message for Failures

Edit "Create WhatsApp Message" node untuk conditional message:

```javascript
{
  "message": `${$json.success ?
    `✅ Build SUCCESS!` :
    `❌ Build FAILED!\n\nError Details: Check GitHub Actions logs`
  }`
}
```

### Send to Multiple Targets

Edit "Send WhatsApp Message" node:

```javascript
{
  "to": "{{ $env.WHATSAPP_TARGET_NUMBER }},{{ $env.WHATSAPP_TARGET_GROUP }}"
}
```

### Add Build Artifacts Links

Modify Extract GitHub Data untuk fetch artifacts:

```javascript
{
  "artifacts_url": "https://api.github.com/repos/{{ owner }}/{{ repo }}/actions/runs/{{ run_id }}/artifacts"
}
```

### Store Build History

Add database node untuk logging:

```javascript
{
  "operation": "insert",
  "table": "vuboi_builds",
  "data": {
    "timestamp": $json.timestamp,
    "status": $json.status,
    "branch": $json.branch,
    "commit": $json.head_commit,
    "whatsapp_sent": true
  }
}
```

---

## 🔐 Security Best Practices

### 1. Environment Variables

**DON'T:**
```javascript
"Authorization": "Bearer my-api-key-123" // ❌ Hardcoded
```

**DO:**
```javascript
"Authorization": "Bearer {{ $env.WHATSAPP_API_KEY }}" // ✅ Environment variable
```

### 2. GitHub Actions Secrets

Store sensitive values di GitHub Secrets:

```yaml
- name: Send Notification
  env:
    N8N_WEBHOOK_URL: ${{ secrets.N8N_WEBHOOK_URL }}
    WHATSAPP_API_KEY: ${{ secrets.WHATSAPP_API_KEY }}
```

### 3. Webhook Security

Enable authentication di webhook:

- Header Auth (recommended)
- Bearer token
- IP whitelist

### 4. API Rate Limiting

Set rate limit di n8n:

```
Max requests: 100 per minute
```

---

## 🐛 Troubleshooting

### WhatsApp Message Not Received

1. Check n8n execution logs
2. Verify API Key valid
3. Check target number format (should be E.164: +62812345678)
4. Test API directly:
   ```bash
   curl -X POST https://your-whatsapp-api.com/send \
     -H "Authorization: Bearer YOUR_KEY" \
     -H "Content-Type: application/json" \
     -d '{
       "to": "62812345678",
       "body": "Test message",
       "type": "text"
     }'
   ```

### GitHub Actions Webhook Not Triggering

1. Verify webhook URL correct
2. Check n8n webhook listening
3. Verify GitHub Actions can reach n8n instance
4. Check firewall/network settings
5. Test webhook manually:
   ```bash
   curl -X POST https://your-n8n.com/webhook/vuboi-token-build \
     -H "Content-Type: application/json" \
     -d '{"test": "data"}'
   ```

### Execution Failed Errors

1. Check node credentials
2. Verify all required fields filled
3. Check logs untuk detailed error
4. Enable debug mode di n8n
5. Test individual nodes

---

## 📊 Advanced: Full Build Pipeline

Untuk lebih lengkap, bisa tambahkan:

### 1. Multi-Channel Notifications
```
n8n Webhook
  ├─ WhatsApp (primary)
  ├─ Slack (backup)
  ├─ Email (archive)
  └─ Database (logging)
```

### 2. Build Analytics
```
Store metrics:
- Build duration
- Success rate
- Platforms generated
- File sizes
```

### 3. Artifact Distribution
```
Auto-upload ke:
- npm registry
- GitHub Releases
- Cloud storage (S3, GCS)
```

### 4. Team Notifications
```
Based on branch:
- main → All team
- develop → Dev team
- feature/* → Author only
```

---

## 📚 Resources

- [n8n Documentation](https://docs.n8n.io)
- [n8n Webhook Node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.webhook/)
- [WhatsApp API Documentation](https://www.whatsapp.com/business/api)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Vuboi Token Project](https://github.com/alfariesh/vuboi-token)

---

**Last Updated:** 2025-10-20
**For:** Vuboi Design Token System
