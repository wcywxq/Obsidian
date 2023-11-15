---
title: 简易 vite 实现
url: https://www.yuque.com/wcywxq/mxunh7/oop1cx
---

```javascript
const Koa = require("koa")
const fs = require("fs")
const path = require("path")
const compilerSfc = require("@vue/compiler-sfc")
const compilerDom = require("@vue/compiler-dom")

const app = new Koa()

function rewriteImports(content) {
    return content.replace(/from ['|"]([^'"]+)['|"]/g, function ($0, $1) {
        // 要访问 node_modules 里的文件
        if ($1[0] !== "." && $1[1] !== "/") {
            return `from '/@modules/${$1}'`
        } else {
            return $0
        }
    })
}

app.use(async ctx => {
    const {
        request: { url, query }
    } = ctx

    if (url == "/") {
        ctx.type = "text/html"
        ctx.body = fs.readFileSync("./index.html", "utf-8")
    }

    if (url.endsWith(".js")) {
        const p = path.resolve(__dirname, url.slice(1))
        const res = fs.readFileSync(p, "utf-8")
        ctx.type = "application/javascript"
        ctx.body = rewriteImports(res)
    }

    if (url.startsWith("/@modules/")) {
        const prefix = path.resolve(__dirname, "node_modules", url.replace("/@modules/", ""))
        const module = require(prefix + "/package.json").module
        const p = path.resolve(prefix, module)
        const res = fs.readFileSync(p, "utf-8")
        ctx.type = "application/javascript"
        ctx.body = rewriteImports(res)
    }

    if (url.includes(".vue")) {
        const p = path.resolve(__dirname, url.split("?")[0].slice(1))
        const { descriptor } = compilerSfc.parse(fs.readFileSync(p, "utf-8"))

        if (!query.type) {
            ctx.type = "application/javascript"
            ctx.body = `
                const __script = ${descriptor.script.content.replace("export default ", "")}
                ${descriptor.styles.length ? `import "${url}?type=style"` : ""}
                import { render as __render } from "${url}?type=template"
                __script.render = __render;
                export default __script;
			`
        }

        if (query.type === "template") {
            const template = descriptor.template
            const render = compilerDom.compile(template.content, {
                mode: "module"
            }).code
            ctx.type = "application/javascript"
            ctx.body = rewriteImports(render)
        }

        if (query.type === "style") {
            const styleBlock = descriptor.styles[0]
            ctx.type = "application/javascript"
            ctx.body = `
                const css = ${JSON.stringify(styleBlock.content)};
                updateStyle(css);
                export default css;
			`
        }
    }
})

app.listen(3000, function () {
    console.log("success listen 3000")
})
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Document</title>
    </head>
    <body>
        <div id="app"></div>
        <script>
            // hash: 规避 shared 文件内的环境判断
            window.process = {
                env: {
                    NODE_ENV: "dev"
                }
            }

            function updateStyle(content) {
                let style = new CSSStyleSheet()
                style.replaceSync(content)
                // @ts-ignore
                document.adoptedStyleSheets = [...document.adoptedStyleSheets, style]
            }
        </script>
        <script type="module" src="./main.js"></script>
    </body>
</html>
```

```javascript
import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')
```

```vue
<template>
    <div>
        <h1>{{ name }}</h1>
        {{ count }}
        <button @click="add">add</button>
    </div>
</template>

<script setup>
  import { ref } from 'vue'
  const name = ref('18zili')
  const count = ref(0)
  const add = () => {
    count.value++
  }
</script>

<style>
#app {
    font-family: "Courier New", Courier, monospace;
}
</style>
```
