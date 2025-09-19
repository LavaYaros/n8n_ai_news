# N8N AI News Agent

This project currently has 1 workflow - it's an agent that reads your newsletter emails and sends brief summaries on topics of interest to your Telegram channel.

For the flow to work, you need to have:
- An email account that you use for receiving newsletters
- A Google Sheet with a list of emails from which you receive the newsletters
- A Telegram channel
- A Telegram bot (as your channel administrator)

## Setup Steps

1. Clone the repository
   ```
   git clone https://github.com/LavaYaros/n8n_ai_news
   ```

2. Set up environment variables
   ```
   copy .env.example .env
   ```
   Instead of `**-***-**-***`, paste your server IP.

   ```
   @"
   N8N_HOST=**-***-**-***.sslip.io
   WEBHOOK_URL=https://**-***-**-***.sslip.io
   N8N_EDITOR_BASE_URL=https://**-***-**-***.sslip.io
   N8N_BASIC_AUTH_USER=your_username
   N8N_BASIC_AUTH_PASSWORD=your_password
   N8N_ENCRYPTION_KEY=generated_encryption_key_32_to_64_characters
   OPENAI_API_KEY=your_openai_api_key
   "@ | Out-File -FilePath .env -Encoding UTF8
   ```

3. Create Caddyfile:
   ```
   @"
   **-***-**-***.sslip.io {
     encode gzip
     reverse_proxy n8n:5678
   }
   "@ | Out-File -FilePath Caddyfile -Encoding UTF8
   ```

4. Create data directory
   ```
   New-Item -ItemType Directory -Path ./n8n_data -Force
   # Note: Ownership change may not be applicable on Windows
   ```

5. Start Docker image
   ```
   docker compose up -d
   ```

6. Copy the Workflow File to the Container
   ```
   docker compose cp workflows/ai_news_agent.json n8n:/tmp/ai_news_agent.json
   ```

7. Import the Workflow
   ```
   docker compose exec -u node n8n n8n import:workflow --input /tmp/ai_news_agent.json
   ```

8. Go to the link you set in Caddyfile

9. Create an owner account

10. Set up auth keys, API keys

11. Activate the workflow
