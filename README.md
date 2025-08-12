# Coomer API Fast ([Try Now](https://colab.research.google.com/github/profinderbro/coomer-api-fast/blob/main/coomer-api-fast.ipynb))

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