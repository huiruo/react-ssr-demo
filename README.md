# react-ssr-demo
```bash
yarn dev
```

## older
```js
function render(req, res, assets) {
  const html = renderToString(<App />);
  console.log('html:', html);
  res.statusCode = "200";
  res.setHeader("Content-Type", "text/html; charset=utf-8");
  res.send(
    `<!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>ssr</title>
    </head>
    <body>
      <div id="root">${html}</div>
      <script src="${assets["main.js"]}"></script>
    </body>
    </html>`
  );
}
```

## new
```js
function newRender(req, res, assets) {
  const { pipe } = renderToPipeableStream(
    <StaticRouter location={req.url}>
      <AppPage />
    </StaticRouter>,
    {
      bootstrapScripts: [assets["main.js"]],
      onShellReady() {
        res.statusCode = 200;
        res.setHeader("Content-Type", "text/html; charset=utf-8");
        res.write(`<!DOCTYPE html>
      <html lang="en">
      <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>ssr</title>
      </head>
      <body>
        <div id="root">`);
        pipe(res);
        res.write(`</div>
    </body>
    </html>`);
      },
    }
  );
}
```