# vite-plugin-react-autoroutes

一个仿照[umiJs](https://v3.umijs.org/zh-CN/docs/routing)的路由方案

## Getting Started

```js
// npm
npm install -D vite-plugin-react-autoroutes

// yarn
yarn install -D vite-plugin-react-autoroutes

// pnpm
pnpm install -D vite-plugin-react-autoroutes
```

## Vite Config

Add to your `vite.config.js`:
```js
import autoRoute from 'vite-plugin-react-autoroutes';

export default {
  plugins: [
    // ...
    autoRoute({{ pagesDir: 'src/pages/' }}),
  ],
}
```

## Router.tsx

`src/router.tsx`

```tsx
import React, { lazy, Suspense, useEffect } from 'react';
import { BrowserRouter as Router, useRoutes, RouteObject } from 'react-router-dom';
import routes, { parse } from 'react-auto-routes';

const syncRouter = (table: SyncRoute.Routes[]): RouteObject[] => {
  let mRouteTable: RouteObject[] = [];
  table.forEach((route) => {
    mRouteTable.push({
      path: route.path,
      element: (
        <Suspense
          fallback={<>Loading</>}
        >
          <route.element />
        </Suspense>
      ),
      children: route.children && syncRouter(route.children),
    });
  });
  return mRouteTable;
};

const Routes = () => {
  return useRoutes(syncRouter(parse(routes, lazy)));
};

const SetRoutes = () => {

  return (
    <Router>
      <Routes />
    </Router>
  );
};

export default SetRoutes
```

