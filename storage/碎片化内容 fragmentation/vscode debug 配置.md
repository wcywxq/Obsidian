---
title: vscode debug 配置
url: https://www.yuque.com/wcywxq/euxgiy/kkkdxxhl1tgpogsf
---

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "chrome",
            "request": "launch",
            "name": "Launch Chrome against localhost",
            "url": "http://localhost:8080",
            "webRoot": "${workspaceFolder}/packages/skyview-web",
            "sourceMapPathOverrides": {
                "webpack:/*": "${webRoot}/*"
            }
        }
    ]
}
```
