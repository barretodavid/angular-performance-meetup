# Lazy Loading

# Rangle.io

---

## What is Lazy Loading?

Ability to load modules on demand => Useful to reduce the app startup time

![Lazy Loading](content/images/lazy-loading.svg)
<!-- .element: style="width: 750px" -->

(Compare branches `no-lazy-loading` vs `normal-lazy-loading`)

---

## Bundle Sizes Comparison (Prod + AoT)

| File                 | Size (No LL) |   Size (LL) |
| ---                  |         ---: |        ---: | 
| main.bundle.js       |      23.9 KB | **17.4 KB** |
| vendor.bundle.js     |       158 KB |      158 KB |
| other js files       |      49.6 KB |     49.6 KB |
| **Initial Download** |     231.5 KB |      225 KB |     
| 0.chunk.js           |            - |  **9.1 KB** |
| **Total Download**   |     231.5 KB |    234.1 KB |

- Webpack creates a "chunk" for every lazy loaded module
- The file `0.chunk.js` is loaded when the user navigates to `admin`
- The initial download size is smaller with LL
- The total size _over time_ is bigger with LL because of Webpack async loading
- The effect of LL start to be noticeable when the app grows

---

## Boot Time Comparison (Prod + AoT)

| Event              | Time (No LL) |  Time (LL) |
| ---                |         ---: |       ---: | 
| DOM Content Loaded |       3.25 s |     3.11 s |
| Load               |       3.27 s |     3.25 s |
| FMP                |       3.30 s | **3.16 s** |

- Not much difference for an small app
- Just one lazy loaded module with a couple of components
- The impact is noticeable for big apps

---

## Preloading

![Lazy Loading](content/images/lazy-loading-with-preloading.svg)
<!-- .element: style="width: 750px" -->

(Compare branches `normal-lazy-loading` vs `master`)

---

## Enable Preloading

Define the property `preloadingStrategy` in the root module routing

```ts
import { PreloadAllModules } from '@angular/router';

export const routes: Routes = [ ... ];

@NgModule({
  imports: [
    RouterModule.forRoot(routes, { 
      preloadingStrategy: PreloadAllModules
    })
  ],
  exports: [ RouterModule ],
})
export class AppRoutingModule {}
```