# Twitter/X Content Extraction via fxTwitter API

Extract full tweet text and embedded article content from X/Twitter without authentication.

## Endpoint

```
GET https://api.fxtwitter.com/{username}/status/{tweet_id}
```

Example: `https://api.fxtwitter.com/WSInsights/status/2052315987194880405`

## Response Structure

```json
{
  "code": 200,
  "message": "OK",
  "tweet": {
    "url": "https://x.com/WSInsights/status/2052315987194880405",
    "id": "2052315987194880405",
    "text": "",  // plain text version
    "raw_text": { "text": "...", "display_text_range": [0, 23], "facets": [] },
    "author": {
      "screen_name": "WSInsights",
      "name": "华尔街洞见 | WS × AI Era",
      "followers": 41257,
      "description": "...",
      "avatar_url": "...",
      "verification": { "verified": true, "type": "individual" }
    },
    "replies": 9,
    "retweets": 129,
    "likes": 453,
    "bookmarks": 1063,
    "quotes": 15,
    "views": 135171,
    "created_at": "Thu May 07 09:13:56 +0000 2026",
    "article": {  // only present for long-form / article tweets
      "title": "Article Title",
      "preview_text": "First few lines...",
      "content": {
        "blocks": [
          { "key": "5tk32", "text": "Some text", "type": "unstyled", "inlineStyleRanges": [...], "entityRanges": [] },
          { "key": "703n7", "text": "Section Header", "type": "header-two", ... },
          { "key": "721cj", "text": "List item", "type": "unordered-list-item", ... }
        ]
      }
    }
  }
}
```

## Block Types → Markdown Mapping

| block.type | Markdown |
|------------|----------|
| `unstyled` | plain text |
| `header-two` | `## {text}` |
| `header-three` | `### {text}` |
| `unordered-list-item` | `- {text}` |
| `ordered-list-item` | `1. {text}` |
| `code-block` | triple-backtick fenced |

Inline styles in `inlineStyleRanges`: `Bold`, `Italic`, `Code`, `Strikethrough`

## Python Extraction Code

```python
import urllib.request, json

url = f"https://api.fxtwitter.com/{username}/status/{tweet_id}"
req = urllib.request.Request(url, headers={"User-Agent": "Mozilla/5.0"})
with urllib.request.urlopen(req, timeout=15) as resp:
    data = json.loads(resp.read())

tweet = data.get('tweet', {})
article = tweet.get('article', {})

# Basic info
print(f"Author: {tweet['author']['name']} (@{tweet['author']['screen_name']})")
print(f"Engagement: {tweet['likes']} likes, {tweet['retweets']} retweets, {tweet['views']} views")

# Article content
if article:
    print(f"Title: {article.get('title')}")
    blocks = article.get('content', {}).get('blocks', [])
    for block in blocks:
        text = block.get('text', '')
        block_type = block.get('type', '')
        if block_type == 'header-two':
            text = f"\n## {text}"
        elif block_type == 'header-three':
            text = f"\n### {text}"
        elif block_type == 'unordered-list-item':
            text = f"- {text}"
        elif block_type == 'ordered-list-item':
            text = f"1. {text}"
        print(text)
```

## Limitations

- Article `content.blocks` only available for long-form tweets (not short tweets)
- Plain tweet text available in `tweet.text` or `tweet.raw_text.text`
- Embedding tweet media (images, video) not available via this API — use `tweet.media` if present
- Rate limits unknown but generous for read-only access
- The `oembed` endpoint (`publish.twitter.com`) requires SSL handshake which often times out from Chinese servers — prefer `fxtwitter.com`

## Use Case

This technique is part of the side-hustle-exploration Phase A (Gather Intelligence) workflow for analyzing:
- Market trend posts
- Tool/product announcements
- Success stories and case studies
- Community discussions about specific side hustle niches
