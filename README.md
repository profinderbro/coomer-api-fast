# #1 Paste in Console 
_open the coomer profile to scrap and paste this to console (inspect)_

```
const urlParts = location.href.split('/');
const username = urlParts[urlParts.length - 1] || urlParts[urlParts.length - 2];

const service = "onlyfans";
const apiBase = `https://coomer.st/api/v1/${service}/user/${username}/posts?o=`;
let allUrls = [];

async function fetchAll(offset = 0) {
  const res = await fetch(apiBase + offset, {
    headers: {
      "Accept": "text/css",  // bypass trick
      "User-Agent": navigator.userAgent,
      "Referer": location.href
    }
  });

  if (!res.ok) {
    console.log("✅ Finished scraping. Total:", allUrls.length);
    saveTxt();
    return;
  }

  const posts = await res.json().catch(() => []);
  if (!posts.length) {
    console.log("✅ No more posts. Total:", allUrls.length);
    saveTxt();
    return;
  }

  for (const post of posts) {
    if (post.file?.path) {
      allUrls.push("https://img.coomer.st/thumbnail/data" + post.file.path);
    }
    for (const att of (post.attachments || [])) {
      if (att.path) {
        allUrls.push("https://img.coomer.st/thumbnail/data" + att.path);
      }
    }
  }

  console.log(`Fetched offset ${offset}, total so far: ${allUrls.length}`);
  await fetchAll(offset + 50);
}

function saveTxt() {
  if (!allUrls.length) {
    console.log("⚠️ No URLs collected.");
    return;
  }
  const blob = new Blob([allUrls.join("\n")], { type: "text/plain" });
  const url = URL.createObjectURL(blob);
  const a = document.createElement("a");
  a.href = url;
  a.download = `${username}_urls.txt`;
  document.body.appendChild(a);
  a.click();
  document.body.removeChild(a);
  URL.revokeObjectURL(url);
  console.log("💾 Download started:", `${username}_urls.txt`);
}

fetchAll();
```

# #2 Coomer API Fast ([Try Now](https://colab.research.google.com/github/profinderbro/coomer-api-fast/blob/main/coomer-api-fast.ipynb))

A fast async media scraper for OnlyFans content from coomer.su with Jupyter notebook interface.

## Features

- Async downloads for improved speed
- Interactive Jupyter widgets interface
- Support for images and videos
- Custom range selection
- Automatic folder organization

## Requirements

```
aiohttp
nest_asyncio
ipywidgets
```

## Usage

1. Install dependencies:
```bash
pip install aiohttp nest_asyncio ipywidgets
```

2. Run the Jupyter notebook:
```bash
jupyter notebook coomer-api-fast.ipynb
```

3. Configure options:
   - **Media Type**: Images, Videos, or Both
   - **Username**: Target OnlyFans username
   - **Range**: All posts or custom offset range

4. Click "Start Scraping" to begin download

## Output

Files are saved to folders named: `{username}_{media_type}_{range}`

## Note

This tool is for educational purposes. Respect content creators' rights and platform terms of service.
