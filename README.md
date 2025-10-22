#🚀 LinkedIn Content Studio — n8n Automation

Fully automated LinkedIn content creation and publishing workflow using n8n + Nano-Banana AI + Airtable integration — designed for SMEs and startups to generate consistent, branded posts weekly.

##Table of Contents

1.Project Overview

2.Features

3.Workflow Architecture

4.Node Details

5.Setup & Configuration

6.Usage

7.Future Enhancements

8.License

##1️⃣ Project Overview

Problem:
SMEs and startups struggle to maintain consistent, engaging LinkedIn content. Marketing teams are often stretched thin, posts are irregular, visuals inconsistent, and engagement is low.

Solution:
This n8n workflow automates content generation, storage, and eventual publishing:

Auto-generates 5 LinkedIn posts weekly (text + visuals) using AI.

Stores drafts in Airtable for human approval.

Maintains brand consistency (color, logo, slogan).

Future steps: auto-posting to LinkedIn, analytics retrieval, and alerts.

Current Status:

Workflow up to draft storage in Airtable is fully functional.

Text and images are generated dynamically using AI.

Drafts include all brand information: logo, color, slogan, company name.

##2️⃣ Features

AI-Powered Text Generation: LinkedIn-ready content tailored to brand tone and post goals.

Branded Image Generation: Includes logos, brand colors, and slogans.

Draft Storage: Draft posts stored in Airtable with status tracking.

Human Approval System: Manual approval ensures quality before posting.

Scalable & Extensible: Workflow can later include auto-posting, analytics, and error monitoring.

##3️⃣ Workflow Architecture

[Schedule Trigger] → [Edit Fields] → [Text Generation] → [Image Generation]
       ↓
       [Airtable Draft Storage] → [Manual Approval] → [LinkedIn Posting] → [Analytics]

Highlights:

Node 1: Schedule Trigger — runs workflow weekly.

Node 2: Edit Fields — defines company info, brand assets, prompts.

Node 3: Text Generation — uses Nano-Banana/OpenRouter to create post text.

Node 4: Image Generation — generates branded visuals for posts.

Node 5: Airtable — stores all draft data for human review.

Node 6+: Manual Approval, LinkedIn Posting, Analytics (planned).

##4️⃣ Node Details
##Node 2 — Edit Fields

Collects company data and post prompts:

{
  "company_name": "NanoForge AI",
  "slogan": "Automate Smart. Grow Fast.",
  "brand_color": "#1A73E8",
  "brand_logo": "https://nanoforge.ai/logo.png",
  "post_goals": "Increase brand authority and engagement",
  "tone": "Friendly, confident, inspiring",
  "hashtags": "#AI #Automation #Startup #Productivity",
  "prompts": [
    "Share a recent customer success story",
    "Give an actionable AI automation tip",
    "Announce a new feature",
    "Share a thought-provoking insight",
    "Highlight a behind-the-scenes moment"
  ],
  "post_count": 5
}

##Node 3 — Text Generation

Generates post content dynamically.

Output path for first post:
  {{$node["Generate text (OpenRouter)"].json["choices"][0]["message"]["content"]}}
Produces LinkedIn-style content: hook, 1–2 paragraphs, CTA, hashtags.

###Node 4 — Image Generation

Uses AI to generate branded visuals.

Pulls brand info dynamically from Edit Fields node:
"prompt": "Create a LinkedIn-style visual based on this post text: {{$node['Generate text (OpenRouter)'].json['choices'][0]['message']['content']}}. Use brand color {{$node['Edit Fields'].json['brand_color']}}, logo from {{$node['Edit Fields'].json['brand_logo']}}, style consistent with slogan '{{$node['Edit Fields'].json['slogan']}}'."

###Node 5 — Airtable Draft Storage

Stores generated text + image + brand info.

Fields mapping example:

| Field         | Value                |
| ------------- | -------------------- |
| post_id       | `{{$now}}`           |
| post_text     | Text from Node 3     |
| image_url     | URL from Node 4      |
| status        | `"Pending Approval"` |
| scheduledDate | `null`               |
| brand_logo    | From Node 2          |
| brand_color   | From Node 2          |
| slogan        | From Node 2          |
| company_name  | From Node 2          |

##5️⃣ Setup & Configuration

Nano-Banana / OpenRouter API

Get your API key, add to n8n credentials.

Airtable

Free account: https://airtable.com/signup

Create base: LinkedIn Content Studio, table: Drafts

Add API key and Base ID in n8n node.

.env Example

NANOBANANA_API_KEY=
AIRTABLE_API_KEY=
AIRTABLE_BASE_ID=
LINKEDIN_ACCESS_TOKEN=
TIMEZONE=UTC


6️⃣ Usage

Trigger workflow via schedule or manually.

Workflow generates 5 draft posts per week.

Drafts stored in Airtable → status "Pending Approval".

Human reviewer approves each post.

Later nodes (LinkedIn + analytics) will handle posting and metrics.

##7️⃣ Future Enhancements

Manual Approval Node → automate notifications for reviewers.

LinkedIn Auto-Posting → schedule approved posts directly.

Analytics Node → fetch engagement metrics.

Error Handling / Alerts → Slack/email notifications for failures or low-confidence AI output.

Logo & Brand Validation → ensure brand consistency before storing draft.

##8️⃣ License

MIT License — feel free to use, modify, and extend this workflow for your own LinkedIn content automation projects.

