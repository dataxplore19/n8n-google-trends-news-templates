# n8n Templates for Google Trends and Google News

Ready-to-import n8n workflows that bring Google Trends and Google News into your automations: trend alerts, news monitoring and a daily AI news digest, sent to Telegram. No code to write: import the JSON file, add two credentials, set your keywords and activate.

The data comes from two Apify Actors: [Google Trends Scraper](https://apify.com/data_xplorer/google-trends-fast-scraper) and [Google News Scraper](https://apify.com/data_xplorer/google-news-scraper-fast). They are called with n8n's built-in HTTP Request node, so no community node is needed.

> Click **Watch > Custom > Releases** at the top of this page to be notified of new templates.

## Templates

| Workflow | What it does | File |
| --- | --- | --- |
| Google Trends alert | Every hour, checks Google's trending searches in a country and sends a Telegram message when one matches your watch words. Each term is sent once. | [`google-trends-alert-telegram.json`](workflows/google-trends-alert-telegram.json) |
| Google News monitoring | Every hour, searches Google News for your keywords (brand, competitors, topics) and sends each new article to Telegram. Each article is sent once. | [`google-news-monitoring-telegram.json`](workflows/google-news-monitoring-telegram.json) |
| Daily AI news digest | Every morning, gets the last 24 hours of Google News for your keywords with the full article text, has an OpenAI model write a short digest, and sends it to Telegram. | [`google-news-ai-digest-telegram.json`](workflows/google-news-ai-digest-telegram.json) |

## Setup

1. **Import**: in n8n, open a new workflow, click the three dots at the top right, choose **Import from File** and select the JSON file. Or copy the file's content and paste it into the canvas.
2. **Apify credential**: create a free [Apify](https://apify.com) account and copy your API token from [Settings > API & Integrations](https://console.apify.com/settings/integrations). In the **Run ... (Apify)** node, create a **Header Auth** credential with name `Authorization` and value `Bearer your_apify_token`.
3. **Telegram credential**: create a bot with [@BotFather](https://t.me/BotFather), then add a **Telegram** credential with its token in the Telegram node. Put your chat ID in the **Settings** node (send a message to [@userinfobot](https://t.me/userinfobot) to get it).
4. **OpenAI credential** (AI digest only): add your OpenAI API key in the **OpenAI Chat Model** node. Any other chat model node can replace it.
5. **Settings**: edit the **Settings** node (keywords, country, watch words), run the workflow once with **Test workflow**, then **Activate** it.

## Settings of each workflow

**Google Trends alert**

- `country`: country code (`US`, `GB`, `FR`, `DE`, `IN`... 125 countries)
- `watchWords`: comma-separated words, matched as whole words in the trending term and its related queries
- `hours`: look-back window of the trending list (`4`, `24`, `48` or `168`)

**Google News monitoring**

- `keywords`: comma-separated search terms. Google operators work (`"exact phrase"`, `site:reuters.com`, `-word`)
- `region`: country and language, such as `US:en`, `GB:en`, `FR:fr`, `DE:de`
- `timeframe`: `1h` for an hourly schedule, `1d` for a daily one

**Daily AI news digest**

- `keywords`, `region`: as above
- `maxArticles`: articles per keyword sent to the model
- `language`: language of the digest

## Use Google Trends and Google News in an n8n AI Agent

For an AI Agent node that decides by itself when to look up trends or news, add an **MCP Client Tool** node with the endpoint `https://mcp.apify.com?tools=data_xplorer/google-trends-fast-scraper,data_xplorer/google-news-scraper-fast` and a Bearer authentication with your Apify token. The agent then gets one tool per Actor.

## Other ways to use the same data

- MCP servers for Claude, Cursor and VS Code: [google-trends-mcp-server](https://github.com/dataxplore19/google-trends-mcp-server), [google-news-mcp-server](https://github.com/dataxplore19/google-news-mcp-server)
- Python: [google-trends-api-python](https://github.com/dataxplore19/google-trends-api-python), [google-news-api-python](https://github.com/dataxplore19/google-news-api-python)
- Node.js: [google-trends-api-node](https://github.com/dataxplore19/google-trends-api-node), [google-news-api-node](https://github.com/dataxplore19/google-news-api-node)

## FAQ

### Do I need a community node?

No. The workflows call the Apify API with the standard HTTP Request node, so they work on n8n Cloud and self-hosted n8n.

### Why use Apify instead of the Google News RSS feed in n8n?

The RSS feed stops at about 100 items, gives news.google.com redirect links instead of the article URL, and has no article text. The Actor returns the publisher URL, the description and the full text.

### Can I send the alerts to Slack, Discord or email instead of Telegram?

Yes. Replace the Telegram node with a Slack, Discord, Gmail or Send Email node and map the `text` field.

## Disclosure

I am the author of the two Apify Actors used by these workflows.

## License

MIT
