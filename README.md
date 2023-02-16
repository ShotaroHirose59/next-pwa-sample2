This is a [Next.js](https://nextjs.org/) project bootstrapped with [`create-next-app`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app).

## Getting Started

First, run the development server:

```bash
npm run dev
# or
yarn dev
```

## next-pwaのインストール
```
npm install next-pwa
```

## manifestの生成
`public`フォルダにPWAの設定ファイルである`manifest.json`を設置

```manifest.json
{
  "theme_color": "#000000",
  "background_color": "#000000",
  "display": "standalone",
  "scope": "/",
  "start_url": "/",
  "name": "next sample pwa",
  "short_name": "next pwa",
  "description": "This is a demo to create PWA with next",
  "icons": [
    {
      "src": "/icon.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/icon.png",
      "sizes": "256x256",
      "type": "image/png"
  },
  {
      "src": "/icon.png",
      "sizes": "384x384",
      "type": "image/png"
  },
  {
      "src": "/icon.png",
      "sizes": "512x512",
      "type": "image/png"
  }
  ]
}
```

## _document.tsxファイルを作成

``` _document.tsx
import Document, { Html, Head, Main, NextScript } from "next/document";

class MyDocument extends Document {
  render() {
    return (
      <Html>
        <Head>
          <link rel="manifest" href="/manifest.json" />
          <link rel="apple-touch-icon" href="/icon.png"></link>
          <meta name="theme-color" content="#fff" />
        </Head>
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    );
  }
}

export default MyDocument;
```

## next.config.js
``` next.config.js
const withPWA = require("next-pwa");

module.exports = withPWA({
  pwa: {
    dest: "public",
    register: true,
    skipWaiting: true,
  },
  reactStrinctMode: true,
});
```

## 補足
### .gitignoreの設定
PWAの起動が成功すると以下のの5つがpublicフォルダ内に生成される。
- sw.js
- sw.js.map
- vercel.svg
- workbox***.js
- workbox***.js.map
これらのファイルは恒久的に更新がされてしまう為、GitHubへはpushしないようにする。


## iOSのスプラッシュスクリーン対応
AndriodではデフォルトでPWAの起動画面をいい感じにしてくれるが、（本サイトでは未検証）
iOSでは起動画面が真っ白になるので別途設定する必要がある。

`_document.tsx`に以下2行を追加

```
<meta name="apple-mobile-web-app-capable" content="yes" />
<script async src="https://cdn.jsdelivr.net/npm/pwacompat@2.0.9/pwacompat.min.js" integrity="sha384VcI6S+HIsE80FVM1jgbd6WDFhzKYA0PecD/LcIyMQpT4fMJdijBh0I7Iblaacawc" crossOrigin="anonymous" />
```

### 実機で確認
- iOSのスプラッシュスクリーン未対応： https://next-pwa-sample-sooty.vercel.app/
- iOSのスプラッシュスクリーン対応済： https://next-pwa-sample2.vercel.app/

<img width="205" alt="スクリーンショット 2023-02-16 18 23 16" src="https://user-images.githubusercontent.com/55835461/219333841-3639cffb-2b05-41cd-ae0b-0dc4d7ea9980.png">
