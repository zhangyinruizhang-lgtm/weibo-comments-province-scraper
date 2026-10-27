# Weibo Comments & China Province Geolocation Scraper (微博评论与IP属地采集 — No Cookie)

> 🚀 **Run Directly on Apify Cloud (100% Cookie-Free, Instant JSON/CSV Export):**
> 👉 **[Launch Weibo Comments & Province Scraper on Apify Store](https://apify.com/axon-studio/weibo-comments-scraper)**

Extract reader comment threads (`post_comments`), comment like counts, sentiment polarity, and **commenter IP province locations (`regionEn`)** from **Sina Weibo (新浪微博)** — **with 100% no login, no cookies, and no VPN required**.

Built for **Chinese consumer sentiment analysis, regional market-share mapping (province-level geolocation), campaign reception audits, and multi-turn LLM dialogue datasets** (n8n, Make, Dify, Claude MCP). Users on the **Apify Free plan ($5 monthly credit) can extract up to 1,111 Weibo comments per month for free**.

---

## What Can This Weibo Comments Scraper Do? (Features)

| Weibo Comments Scraper Capability | Built-In Advantage |
| :--- | :--- |
| **Dual-Engine Comment Extraction (`post_comments`)** | Queries both PC Ajax (`/ajax/statuses/buildComments`) and Mobile H5 (`/api/comments/show`) endpoints automatically so comment threads are never missed. |
| **Smart Keyword Auto-Discovery** | Don't have specific Post IDs? Leave `postIds` empty and enter any English or Chinese keyword in `searchQuery` (e.g., `Tesla`, `Luckin`, `瑞幸咖啡`) to automatically find active posts and harvest their comments! |
| **IP Province Geolocation (`regionEn`)** | Automatically translates every commenter's Chinese IP location tag (`来自广东`, `来自上海`) into standardized English province and country names (`Guangdong`, `Shanghai`, `Beijing`, `United States`). |
| **Built-In Chinese Sentiment Analysis** | Tags every comment with `sentiment` (`positive` / `negative` / `neutral`) and `sentimentScore` (`-1.0` to `+1.0`), accurately handling Chinese negation reversal (`"不好看"` → negative, `"不错"` → positive). |
| **Single Post Engagement Refresh (`post_details`)** | Switch `mode` to `post_details` to quickly inspect the parent post's full text, `attitudesCount` (likes), `commentsCount`, and `repostsCount`. |
| **256 MB RAM Real-Time Streaming** | Streams comment records into your Dataset in real-time batches of 10 rows on just **256 MB RAM** with automatic comment ID deduplication (`deltaMode`). |

---

## How to Scrape Weibo Comments in 3 Easy Steps

1. **Provide Target Posts OR a Keyword** — Paste one or more Weibo 16-digit Post IDs, `mblogid` codes, or URLs into **`postIds`**; OR leave `postIds` empty and type a brand/topic in **`searchQuery`** (e.g., `Tesla`) for automatic post discovery.
2. **Set `maxResults`** — Choose the total number of comment rows to extract (`1` to `5,000`).
3. **Click "Start"** — Export your comment dataset with English province locations and sentiment scores to **JSON, CSV, Excel, or Parquet**, or connect via API / Claude MCP.

---

## Weibo Comments Scraper Use Cases

| Buyer Profile | Weibo Comment & Geolocation Use Case |
| :--- | :--- |
| **Brand & Consumer Insights Teams** | The post is the prompt; the replies are the true consumer sentiment. Measure authentic buyer reactions under official product launch posts or KOL reviews |
| **Regional Marketing & Retail Analysts** | Map where in China the conversation is happening using `regionEn` (`Guangdong`, `Zhejiang`, `Sichuan`, `Beijing`) to guide regional ad spend and store openings |
| **AI / LLM Dialogue & SFT Teams** | Harvest authentic colloquial Chinese multi-turn replies and internet slang with like-count quality signals (`likesCount`) |
| **PR Crisis & Public Opinion Desks** | Track real-time public backlash or support in the comment sections of breaking trending posts |

---

## Input

All parameters are configured in the **Input** tab. Below are the main input fields and JSON examples:

| Input Field | Type | Description |
| :--- | :--- | :--- |
| **`postIds`** | `array` | Weibo 16-digit post IDs (`"5356654347420790"`), `mblogid` codes (`"RnLPYv8ua"`), or post URLs. Leave empty `[]` to auto-discover posts via `searchQuery`. |
| **`searchQuery`** | `string` | Brand or topic in English or Chinese (e.g., `"Tesla"`, `"瑞幸咖啡"`), used to auto-discover active comment threads when `postIds` is empty. |
| **`autoLocalize`** | `boolean` | Automatically translates English keywords into Simplified Chinese during auto-discovery (default: `true`). |
| **`maxResults`** | `integer` | Maximum number of comment records to return (`1` to `5,000`, default: `100`). |
| **`sentimentAnalysis`** | `boolean` | Adds `sentiment` (`positive` / `negative` / `neutral`) and `sentimentScore` (`-1.0` to `+1.0`) to each comment (default: `true`). |
| **`deltaMode`** | `boolean` | Remembers scraped `commentId` values across scheduled runs so you only pay for new replies (default: `false`). |
| **`mode`** | `string` | `"post_comments"` (default) to extract comments, or `"post_details"` to extract parent post metrics. |

### 1. Extract Comments from Specific Weibo Post IDs or URLs (JSON)
```json
{
  "mode": "post_comments",
  "postIds": [
    "5356654347420790",
    "https://weibo.com/1749127163/RnLKq8wB2"
  ],
  "maxResults": 200,
  "sentimentAnalysis": true,
  "deltaMode": false
}
```

### 2. Auto-Discover Active Posts & Harvest Comments by Brand Keyword (JSON)
```json
{
  "mode": "post_comments",
  "postIds": [],
  "searchQuery": "Tesla",
  "autoLocalize": true,
  "maxResults": 150,
  "sentimentAnalysis": true
}
```

---

## Output

Each extracted comment is stored in your Apify **Dataset** with standardized province geolocation and sentiment fields (matching the `"5356654347420790"` input above):

```json
{
  "mode": "post_comments",
  "postId": "5356654347420790",
  "commentId": "5356658192014412",
  "text": "焕新版 Model Y 的隔音和底盘滤震确实提升很大，周末刚去广州门店试驾完！",
  "sentiment": "positive",
  "sentimentScore": 0.78,
  "createdAt": "Sat Oct 24 14:22:10 +0800 2026",
  "createdAtIso": "2026-10-24T14:22:10+08:00",
  "authorId": "6108685154",
  "authorName": "数码玩家Leo",
  "authorVerified": false,
  "regionName": "广东",
  "regionEn": "Guangdong",
  "likesCount": 42,
  "scrapedAt": "2026-10-24 16:45:10 UTC"
}
```

### Output Field Definitions
* **`postId`**: The 16-digit ID of the parent Weibo post (use this to join comments back to posts).
* **`commentId`**: Unique ID of the comment (deduplicated within and across runs).
* **`text`**: Clean HTML-stripped comment text in Simplified Chinese.
* **`regionName` / `regionEn`**: Commenter's IP location as displayed by Weibo (`广东` → `"Guangdong"`).
* **`likesCount`**: Number of upvotes/likes the comment has received.
* **`sentiment` / `sentimentScore`**: Sentiment polarity (`positive`, `negative`, `neutral`) and score (`-1.0` to `+1.0`).

---

## Automated Comment Thread Monitoring (Schedules & Delta Mode)

Monitor viral posts for new reader replies automatically:
1. In **Apify Console → Schedules → Create new schedule**, select **`axon-studio/weibo-comments-scraper`** and set your run interval.
2. Enable **`deltaMode: true`**. Previously delivered `commentId` records are stored in `DELTA_SEEN_IDS` and skipped automatically on future runs—so you only pay for brand-new comments.

### Python & Synchronous REST API Integration

```python
from apify_client import ApifyClient

client = ApifyClient("YOUR_APIFY_API_TOKEN")
run = client.actor("axon-studio/weibo-comments-scraper").call(run_input={
    "mode": "post_comments",
    "postIds": ["5356654347420790"],
    "maxResults": 100,
    "sentimentAnalysis": True,
})
for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(item["commentId"], item["regionEn"], item["sentiment"], item["text"])
```

```bash
curl -X POST "https://api.apify.com/v2/acts/axon-studio~weibo-comments-scraper/run-sync-get-dataset-items?token=YOUR_APIFY_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"mode":"post_comments","postIds":[],"searchQuery":"Tesla","maxResults":50}'
```

---

## How Much Does It Cost to Scrape Weibo Comments? (Pricing)

**$4.50 per 1,000 comments (`$0.0045` per comment) + `$0.02` per Actor start (Pay-Per-Event)**

* **Free Plan Credits**: Every Apify account includes **$5.00 in free monthly credits**, covering up to **1,111 Weibo comments every month for free**.
* **Volume Discounts**: Automatic lower unit prices for Apify **Bronze / Starter**, **Silver / Scale**, and **Gold / Business** plan subscribers.
* **No Server Compute Fees**: Memory and CPU usage are included in the Pay-Per-Event price.
* **Typical Run Costs**: 100 comments = **`$0.47`** · 500 comments = **`$2.27`** · 2,000 comments = **`$9.02`**.

---

## FAQ & Legal Compliance

### Do I need a Weibo login cookie to extract comments and IP provinces?
**No.** This Actor uses a dual PC and Mobile comment engine paired with automatic session management—no personal cookies or login required.

### Is it legal to scrape public Weibo comments?
This Actor extracts publicly visible comments and IP province tags displayed on Weibo posts and does not access private data. Public usernames, UIDs, and province labels may constitute personal data under **GDPR** or **PIPL**; ensure you have a lawful basis for market research or NLP analysis. *Not affiliated with Sina Corporation or Weibo.*

---

## More Chinese Social Intelligence Scrapers by Axon Studio

* **[Weibo Scraper — All-in-One Search, Hot Trends, Comments & BlackCat](https://apify.com/axon-studio/weibo-scraper)** — 9-mode comprehensive Sina Weibo & BlackCat intelligence suite.
* **[BlackCat Scraper 黑猫投诉 — China Consumer Complaints API](https://apify.com/axon-studio/blackcat-scraper)** — Extract consumer disputes and brand complaints from `tousu.sina.com.cn`.
* **[Weibo User Posts & Profile Scraper (No Cookie)](https://apify.com/axon-studio/weibo-user-posts-scraper)** — Extract creator timelines (`user_posts`) and KOL follower metrics without login.
