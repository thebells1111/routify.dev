---
layout: default
---

<script>
    import { Tabs, TabsLink, TabsPage } from "@sveltech/bricks";
</script>

# Express example

#### 1. install express and ssr as dependencies
```
npm i express tossr
```

#### 2. install vite-main-js as dev dependency
```
npm i vite-main-js -D
```

#### 3. add vite-main-js to plugins in `vite.config.js`
```
import { svelte } from "@sveltejs/vite-plugin-svelte";
import { defineConfig } from "vite";
import mainJS from "vite-main-js";

export default defineConfig({
  server: {
    port: 5000,
  },

  plugins: [svelte(), mainJS()],
});
```

#### 4. build dist folder
```
npm run build
```


#### 5. Create `server.js`

<Tabs>
  <div class="c-tabs">
    <TabsLink>Minimal without SSR</TabsLink>
    <TabsLink>Basic with SSR and custom endpoint</TabsLink>

  </div>
  <div class="c-tabs-pages">
    <TabsPage>

      ```javascript
      const express = require('express')
      const app = express()

      const PORT = 5000
      const TEMPLATE = 'dist/index.html'

      // serve assets, if they exist
      app.use(express.static('dist'))

      // otherwise serve Routify
      app.get('*', async (req, res) => {
        res.sendFile(TEMPLATE, { root: __dirname })
      })

      // start server
      app.listen(PORT)
      console.log('serving on port', PORT)

```

    </TabsPage>
    <TabsPage>

      ```javascript
      const { tossr } = require('tossr')
      const express = require('express')
      const app = express()

      const TEMPLATE = 'dist/index.html'
      const SCRIPT = 'dist/build/bundle.js' // dist/build/main.js if you're using dynamic imports
      const PORT = 5000
      const OPTIONS = { inlineDynamicImports: true };

      // serve some custom endpoints
      app.get('/hello', async (req, res) => {
        res.send('hello')
      })

      // else serve assets, if they exist
      app.use(express.static('dist'))

      // else serve Routify
      app.get('*', async (req, res) => {        
        const html = await tossr(TEMPLATE, SCRIPT, req.url, OPTIONS)
        res.send(html)
      })

      // start server
      app.listen(PORT)
      console.log('serving on port', PORT)
```
    </TabsPage>
  </div>
</Tabs>





#### 3. Run server
```
node server
```

---

