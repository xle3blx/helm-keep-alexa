# Helm Keep Alexa Skill

Alexa skill package for [Helm Keep](https://github.com/YOUR_USERNAME/helm-keep-web), a household chore and allowance manager.

## What it does

Kids can interact with their chores using voice commands on any Alexa-enabled device:

- **"Alexa, open My Chores"** — Launch the skill
- **"I'm Jesse"** — Identify which family member is speaking
- **"What are my chores?"** — List assigned chores with due dates and rewards
- **"I finished the dishes"** — Submit a chore for completion/approval
- **"What's my balance?"** — Check current allowance balance

## Setup

### Prerequisites

- A running Helm Keep instance accessible via HTTPS
- An [Amazon Developer](https://developer.amazon.com) account

### 1. Configure your Helm Keep instance

1. Log into Helm Keep as an admin
2. Go to **Settings > Alexa Integration**
3. Click **Generate API Key** and copy the key

### 2. Create the Alexa Skill

#### Option A: Alexa Developer Console (manual)

1. Go to [developer.amazon.com/alexa/console/ask](https://developer.amazon.com/alexa/console/ask)
2. Click **Create Skill** > **Custom** > **Provision your own**
3. Set the invocation name to `my chores`
4. Copy the interaction model from `skill-package/interactionModels/custom/en-US.json` into the **JSON Editor** under the Interaction Model section
5. Under **Endpoint**, choose **HTTPS** and enter: `https://YOUR_DOMAIN/api/alexa`
6. Select the SSL certificate type: **My development endpoint has a certificate from a trusted certificate authority**
7. Under **Account Linking**, enable it with:
   - Authorization Grant Type: **Implicit Grant**
   - Authorization URI: `https://YOUR_DOMAIN/alexa/link`
8. Save and build the model

#### Option B: ASK CLI

1. Update `skill-package/skill.json` — replace `YOUR_DOMAIN` with your Helm Keep URL
2. Install the [ASK CLI](https://developer.amazon.com/en-US/docs/alexa/smapi/quick-start-alexa-skills-kit-command-line-interface.html)
3. Run: `ask deploy`

### 3. Link your account

1. Open the **Alexa app** on your phone
2. Find the Helm Keep skill under **Your Skills > Dev Skills**
3. Tap **Link Account**
4. Paste the API key from step 1
5. You're all set!

### 4. Test it

Say: **"Alexa, open My Chores"**

## How it works

- The skill uses **name-based identification** — one Amazon account (the parent's) links to the household, and kids identify themselves by saying their name
- Member names are fuzzy-matched against household members (case-insensitive, partial matching)
- Chore names are also fuzzy-matched so kids don't need to say exact titles
- The skill respects **Family Mode** — if Family Mode is active, chore completion is blocked
- All task completions go through the normal approval workflow (admin approval, self-report, or peer confirm — depending on your household setting)

## Files

```
skill-package/
  skill.json                              # Skill manifest (endpoint, account linking)
  interactionModels/custom/en-US.json     # Interaction model (intents, utterances)
```

The backend code lives in the [helm-keep-web](https://github.com/YOUR_USERNAME/helm-keep-web) repo under `src/lib/alexa/` and `src/app/api/alexa/`.
