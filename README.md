# Tweaks for the qutebrowser config (WIP)

This is a collection of snippets for use in the qutebrowser config, such as HTTPS-only mode, and something like [Privacy Redirect](https://github.com/SimonBrazell/privacy-redirect).

### HTTPS only mode

Extend the allowlist with values for your customized use.

```python
from PyQt5.QtCore import QUrl
from qutebrowser.api import interceptor

def redirect_80_to_443(info: interceptor.Request):
    url = info.request_url
    scheme = url.scheme()
    req_host = url.host()
    allowlist = [
        "localhost",
        "127.0.0.1"
    ]

    if scheme == "http" and req_host not in allowlist:
        url.setScheme("https")
        try:
            info.redirect(url)
        except:
            pass
        return True

interceptor.register(redirect_80_to_443)
```
