## RPI Proxy

```javascript
function FindProxyForURL(url) {
  const proxyOrigin = '192.168.1.16';
  const proxyPort = 7890;
  const rules = ['*.google.*', '*.github.*'];

  const matched = rules.some(function (rule) {
    return shExpMatch(url, rule);
  });

  if (matched) {
    return 'PROXY ' + proxyOrigin + ':' + proxyPort;
  }

  return 'DIRECT';
}
```
