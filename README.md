# LLM Scraper

LLM Scraper helps you to convert any webpages into structured data using LLMs.

**Features**

- Uses OpenAI chat models
- Schemas defined with Zod
- Full type-safety with TypeScript
- Based on Playwright framework
- Supports 3 operating modes: `html`, `text`, `image`
- Streaming when crawling multiple pages

## Get started

1. Install the required dependencies from npm:

```
npm i zod playwright llm-scraper
```

2. Get an OpenAI API key and set it in your environment variables:

```
export OPENAPI_API_KEY=***
```

## Example

In this example, we're extracting top stories from HackerNews:

```ts
import z from 'zod'
import { chromium } from 'playwright'
import LLMScraper from 'llm-scraper'

// Create a new browser instance
const browser = await chromium.launch()

// Initialize the LLMScraper instance
const scraper = new LLMScraper(browser)

// Define schema to extract contents into
const schema = z.object({
  top: z
    .array(
      z.object({
        title: z.string(),
        points: z.number(),
        by: z.string(),
        commentsURL: z.string(),
      })
    )
    .describe('Top stories on Hacker News'),
})

// URLs to scrape
const urls = ['https://news.ycombinator.com']

// Run the scraper
const pages = await scraper.run(urls, {
  model: 'gpt-4-turbo',
  schema,
  mode: 'html',
  closeOnFinish: true,
})

// Stream the result from LLM
for await (const page of pages) {
  console.log(page.data)
}
```

### Contributing

As an open-source project, we welcome contributions from the community. If you are experiencing any bugs or want to add some improvements, please feel free to open an issue or pull request.
