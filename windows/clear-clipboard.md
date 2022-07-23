## 将清理剪切板功能添加到右键菜单

将下面内容保存到reg文件中， 双击运行

```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\directory\background\shell\清理剪切板]

[HKEY_CLASSES_ROOT\directory\background\shell\清理剪切板\command]
@="cmd /c \"echo off | clip\""
```