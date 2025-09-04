```
const username = "urbabydollxo";
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
