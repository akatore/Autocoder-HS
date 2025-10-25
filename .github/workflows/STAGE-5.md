https://hyperskill.org/projects/458/stages/2648/implement#comment




Easy way to pass this stage without using a "real" OpenAI API key.
- Head over to groq: https://console.groq.com and create an account;
- Generate an API key here: https://console.groq.com/keys;
- Save that key in secrets;
- Open script.sh and change the base URL to: https://api.groq.com/openai/v1/chat/completions;
- Change the model to one supported by Groq such as "llama-3.3-70b-versatile". All supported models here: https://console.groq.com/docs/models;
Your function should look like this: 
```
# Function to send prompt to the ChatGPT model (OpenAI API)
send_prompt_to_chatgpt() {
curl -s -X POST "https://api.groq.com/openai/v1/chat/completions" \
    -H "Authorization: Bearer $OPENAI_API_KEY" \
    -H "Content-Type: application/json" \
    -d "{\"model\": \"llama-3.3-70b-versatile\", \"messages\": $MESSAGES_JSON, \"max_tokens\": 500}"
}
```
The are limits when using this API (view at https://console.groq.com/settings/limits), but for this project, it should suffice. 
Once you are ready to use the real OpenAI APIs, just revert to the original version.


```
name: Autocoder Bot

on:
  issues:
    types: [opened, reopened, labeled]

jobs:
  generate_code:
    if: contains(github.event.issue.labels.*.name, 'autocoder-bot')
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      # Objective: Make the script file executable
      # Note: This assumes your script is named 'gpt_interaction_script.sh'
      # If you named it 'script.sh', change the filename below.
      - name: Set permissions for script.sh
        run: chmod +x scripts/script.sh

      # Objective: Run the script and pass the necessary environment variables
      - name: Run the script to generate code
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          REPOSITORY: ${{ github.repository }}
          ISSUE_NUMBER: ${{ github.event.issue.number }}
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
        run: ./scripts/script.sh "${GITHUB_TOKEN}" "${REPOSITORY}" "${ISSUE_NUMBER}" "${OPENAI_API_KEY}"

      # Objective: Upload the content as an artifact
      - name: Upload autocoder output
        uses: actions/upload-artifact@v4
        with:
          name: autocoder-artifact
          path: autocoder-bot/
          
      # Objective: Download the artifact
      - name: Download artifacts
        uses: actions/download-artifact@v4
        with:
          name: autocoder-artifact
          path: autocoder-artifact/  # Download to this directory

      - name: Display generated files
        run: ls -R ./autocoder-artifact




```
