# Publication record

## learn/mcp → tvijo.com — 2026-10-29, 12:23–12:26 UTC

| | |
| --- | --- |
| Public URL | <https://tvijo.com/learn/mcp/> |
| RU URL | <https://tvijo.com/learn/mcp/index.ru.html> |
| CSS | <https://tvijo.com/learn/mcp/style.css> |
| Host | `newsagr` (204.168.175.142), SSH alias `newsagr` |
| Files on host | `/var/www/tvijo-mcp-learn-20261029-7b31/` |
| nginx snippet | `/etc/nginx/snippets/tvijo-mcp-learn-20261029-7b31.conf` |
| Include | line 49 of `/etc/nginx/sites-available/tvijo.com` (the live vhost is the symlink `sites-enabled/tvijo.com`) |
| Config backup | `/etc/nginx/sites-available/tvijo.com.bak-20261029-mcp-learn` |

### Verification actually run

```
curl -s -o /dev/null -w '%{http_code}' https://tvijo.com/learn/mcp/            -> 200
curl -s -o /dev/null -w '%{http_code}' https://tvijo.com/learn/mcp/index.ru.html -> 200
curl -s -o /dev/null -w '%{http_code}' https://tvijo.com/learn/mcp/style.css    -> 200
```

Byte sizes served match the repository files exactly: `index.html` 19,650 · `index.ru.html`
23,521 · `style.css` 12,680. Titles read back correctly in EN and RU. Response carries the
scoped `Content-Security-Policy` (`style-src 'self'`, `script-src 'none'`) and
`X-Robots-Tag: noindex, nofollow`.

### Test mode

The page is published **with `noindex, nofollow`**. It is not in `sitemap.xml` and is not
linked from the home page. Indexing is switched on by removing the `X-Robots-Tag` line in the
three HTML locations of the snippet and reloading nginx — one decision, not a redeploy.

### Rollback

```sh
# on newsagr
sed -i '/tvijo-mcp-learn-20261029-7b31/d' /etc/nginx/sites-available/tvijo.com
nginx -t && systemctl reload nginx
rm -f /etc/nginx/snippets/tvijo-mcp-learn-20261029-7b31.conf
rm -rf /var/www/tvijo-mcp-learn-20261029-7b31
```

The backup file restores the exact previous vhost if anything else was touched.

### Defect found and fixed during publication

First attempt used `alias … /index.html` inside `location = /learn/mcp/`. nginx appended its
index again and logged `"/var/www/.../index.htmlindex.html" is not a directory` → HTTP 500.
Fixed by aliasing the directory and setting `index index.html`. Recorded because the same
mistake will be made again by anyone repeating this pattern.

### Not done, on purpose

- No sitemap entry, no internal links from the home page (test mode).
- No analytics: the snippet forbids scripts (`script-src 'none'`).
- Existing `tvijo.com` content, snippets and headers untouched; only one `include` line added.
