# Research Aggregator & Scoring Workflow - n8n Automation

## Overview
This repository contains the JSON configuration for the **Research Aggregator & Scoring Workflow**, an n8n workflow that aggregates information from multiple public sources (Wikipedia, Google News RSS, and YouTube videos) based on a user-provided topic. It summarizes key insights, scores each source on interest level, content relevance, and data quality, and appends the results to a Google Sheet for easy tracking and analysis.

## Workflow Description
The workflow automates research aggregation and scoring by:
1. **Form Submission**: Accepts a topic or keyword from the user via a form.
2. **Topic Encoding**: Prepares the topic for URL-safe queries.
3. **YouTube Search**: Retrieves up to 3 relevant videos, extracting titles and descriptions.
4. **Wikipedia Summary**: Fetches a concise summary from Wikipedia.
5. **Google News RSS**: Pulls recent news articles via RSS feed and selects the top 5.
6. **Data Merging**: Combines processed data from all sources.
7. **AI Agent Processing**: Uses OpenAI to summarize insights, score sources (interest, relevance, quality), identify themes, and provide recommendations.
8. **Output Parsing**: Cleans AI output into structured JSON.
9. **Sheet Append**: Adds the summary, insights, scores, and recommendations to a Google Sheet.

## Workflow Structure
The workflow includes the following key nodes:
- **On form submission** (`n8n-nodes-base.formTrigger`): Captures user input (topic).
- **Code** (`n8n-nodes-base.code`): Encodes the topic for queries.
- **Get many videos** (`n8n-nodes-base.youTube`): Searches YouTube for videos.
- **Edit Fields2** (`n8n-nodes-base.set`): Formats YouTube data.
- **Wikipedia** (`n8n-nodes-base.httpRequest`): Fetches Wikipedia summary.
- **Edit Fields1** (`n8n-nodes-base.set`): Extracts Wikipedia content.
- **RSS Read** (`n8n-nodes-base.rssFeedRead`): Retrieves Google News RSS.
- **Code2** (`n8n-nodes-base.code`): Sorts and formats top 5 news articles.
- **Merge** (`n8n-nodes-base.merge`): Combines data from all sources.
- **OpenAI Chat Model** (`@n8n/n8n-nodes-langchain.lmChatOpenAi`): Powers the AI agent.
- **AI Agent** (`@n8n/n8n-nodes-langchain.agent`): Summarizes, scores, and generates insights.
- **Code1** (`n8n-nodes-base.code`): Parses AI output into clean JSON.
- **Append row in sheet** (`n8n-nodes-base.googleSheets`): Adds results to Google Sheet.

## Inputs
- **User Form**: A single field for the topic/keyword (e.g., "open source software").
- **Public APIs**: Queries Wikipedia, Google News RSS, and YouTube for data.

## Outputs
- **Structured JSON**: Includes:
  - `summary`: Concise overview of the topic.
  - `insights`: Bullet points of key findings.
  - `scoring`: Scores for each source (interest, relevance, quality).
  - `overallScore`: Average relevance score.
  - `recommendation`: Suggested follow-up actions.
- **Google Sheet Append**: Adds a new row with the above data.

## Prerequisites
- **n8n Instance**: Active n8n setup (cloud or self-hosted).
- **YouTube API Credentials**: OAuth2 for video searches.
- **OpenAI API Credentials**: For the chat model and AI agent.
- **Google Sheets API Credentials**: OAuth2 for appending rows.
- **Google Sheet**: A sheet named "n8n workflow" with "Sheet1" containing columns like `Topic`, `Summary of the topic`, `Insights`, `Scoring`, `OverallScore`, `Recommendation`.

## Setup Instructions
1. **Import Workflow**:
   - Copy the JSON from `Research Aggregator & Scoring Workflow.json`.
   - Import into n8n via Workflows > Import from File/Clipboard.
2. **Configure Credentials**:
   - Set YouTube OAuth2 for `Get many videos`.
   - Configure OpenAI for `OpenAI Chat Model`.
   - Set Google Sheets OAuth2 for `Append row in sheet`.
3. **Update Node Details**:
   - Ensure RSS and Wikipedia URLs use the encoded topic.
   - Test the form trigger or simulate input.
4. **Test Workflow**:
   - Submit a topic via the form.
   - Verify data aggregation, AI processing, and sheet append.

## Usage
- **Trigger via Form**: Input a topic to start research.
- **Data Flow**: Fetches from sources, merges, AI analyzes/scores, appends to sheet.
- **Customization**: Adjust prompts in the AI agent for different scoring criteria.

## Notes
- The workflow limits YouTube to 3 videos and news to top 5 for efficiency.
- Ensure API quotas are not exceeded; add error handling if needed.
- The JSON parsing in `Code1` assumes clean AI output; adjust if formats vary.

## Contributing
Fork the repo, make changes, and submit a pull request. Suggestions for adding sources or refining AI prompts welcome!

## License
MIT License - see [LICENSE](LICENSE) for details.