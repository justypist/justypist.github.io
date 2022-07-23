# Git proxy

| 参考 `https://www.cnblogs.com/yongy1030/p/11699086.html`

## 设置代理
```bash
git config --global https.proxy 'socks5://192.168.1.16:7890'
git config --global http.proxy 'socks5://192.168.1.16:7890'
```

## 查看代理
```bash
git config --global --get http.proxy
git config --global --get https.proxy
```

## 取消代理
```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```