[搜索引擎优化 (SEO) 新手指南](https://developers.google.com/search/docs/fundamentals/seo-starter-guide?hl=zh-cn)
[Search Engine Optimization](https://nextjs.org/learn/seo/introduction-to-seo)
[Web 指标](https://web.dev/vitals/)
[SEO Writing](https://backlinko.com/hub/seo/seo-writing)
# meta seo 设置
可使用 [https://smo.knowem.com/](https://smo.knowem.com/) 检查 SEO 评分
```jsx
<title>Document</title>
  
<meta name="description" content="本頁描述，不要超過 155 個字元。" />
<meta name="keywords" content="關鍵字1,關鍵字2" />


{/* 尽量不要使用 user-scalable:0 或 maximum-scal 小于 5，会影响 A11y */}
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=5, viewport-fit=cover" />

<meta name="author" content={author} />
<meta name="creator" content={author} />
<meta name="publisher" content={author} />

<meta name="applicable-device" content="pc,mobile" />
<meta name="application-name" content={applicationName} />
{/* 用于设置网站磁贴颜色或 Website App 图标的背景颜色，可以应用于Windows 8和Windows 10操作系统的磁贴功能 */}
<meta name="msapplication-TileColor" content="#ffffff" />
{/* Used by Chrome, Firefox OS, and opera to change the browser address bar. */}
<meta name="theme-color" content={theme.colors.default.theme_color.primary} />
<meta name="theme-color" media="(prefers-color-scheme: light)" content="#fff" />
<meta name="theme-color" media="(prefers-color-scheme: dark)" content="#000" />
<meta name="category" content="分类" />

{/* https://ogp.me/ */}
<meta property="og:title" content={_title} />
<meta property="og:description" content={_description} />
<meta property="og:url" content={_url} />
<meta property="og:site_name" content={_title} />
<meta property="og:locale" content={"zh"} />
<meta property="og:image" content={_image} />
<meta property="og:image:width" content={800} />
<meta property="og:image:height" content={600} />
<meta property="og:image:alt" content={_description} />
<meta property="og:image" content={_image} />
<meta property="og:image:width" content={1800} />
<meta property="og:image:height" content={1600} />
<meta property="og:image:alt" content={_description} />
<meta property="og:type" content="website" />


{/* @see https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/markup */}
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content={_title} />
<meta name="twitter:description" content={_description} />
<meta name="twitter:image" content={_image} />
<meta name="twitter:creator" content={author} />
<meta name="twitter:site:id" content={twitterId} />
<meta name="twitter:creator:id" content={twitterId} />

{/* https://ziyuan.baidu.com/site/index#/ */}
<meta name="baidu-site-verification" content={BA_VERIFICATION} />

{/* 一些旧版的浏览器或搜索引擎可能不支持 SVG 格式的图标。此外，一些搜索引擎可能会从网站中提取 ICO 或 PNG 格式的图标，而不是使用指定的 SVG 文件 */}
{/* 因此，建议在网站中同时提供 ICO、PNG 和 SVG 三种格式的图标 */}
{/* 定义要在搜索结果中显示的网站图标: https://developers.google.com/search/docs/appearance/favicon-in-search?hl=zh-cn#guidelines */}
{/* 生成 favicon 和 meta 标签：https://realfavicongenerator.net/ */}
<link rel="icon" href="/favicon.ico">
<link rel="icon" href="/favicon.svg" type="image/svg+xml">
<link rel="icon" href="_/static/favicons/favicon-16x16.png" type="image/png" sizes="16x16" fetchpriority="low">
<link rel="icon" href="_/static/favicons/favicon-32x32.png" type="image/png" sizes="32x32" fetchpriority="low">
{/* 用于指定在iOS设备上添加到主屏幕时所使用的网站图标 */}
<link rel="apple-touch-icon" href="_/static/favicons/apple-touch-icon.png" sizes="180x180" fetchpriority="low">
{/* 用于指定在Safari浏览器中网站的标签栏和书签上所使用的图标 */}
<link rel="mask-icon" href="_/static/favicons/safari-pinned-tab.svg" fetchpriority="low">

{/* canonical url */}
{/* https://nextjs.org/learn/seo/crawling-and-indexing/canonical */}
{canonical && <link rel="canonical" href={canonical} itemProp="url" />}
{/* noindex robots */}
{/* https://developers.google.com/search/docs/crawling-indexing/robots-meta-tag?hl=zh-cn */}
{noindex && <meta name="robots" content="noindex,nofollow" />}
```

## What are canonical tags?
[https://nextjs.org/learn/seo/crawling-and-indexing/canonical](https://nextjs.org/learn/seo/crawling-and-indexing/canonical)
Canonical Tags 用于指示搜索引擎哪个URL是主要或首选的版本，以避免重复内容和搜索引擎排名问题。如果有多个URL显示相同或相似的内容，这可能会对搜索引擎优化产生负面影响。通过在副本页面中添加rel="canonical"标签，并将其指向主要版本的URL，可以帮助搜索引擎确定哪个页面应该被视为原始版本，从而避免重复问题和优化困境。
```javascript
<link rel="canonical" href={canonicalUrl} itemProp="url" />
```

## Structured Data and JSON-LD
Structured data facilitates the understanding of your pages to search engines. Over the years, there have been several vocabularies in place, but currently the main one is [schema.org](https://schema.org/).
According to the website, schema.org is a "collaborative, community activity with a mission to create, maintain, and promote schemas for structured data on the Internet, on web pages, in email messages, and beyond."
Schema.org's vocabulary can be used with many different encodings, including [RDFa](https://www.w3.org/TR/rdfa-primer/), [Microdata](https://www.w3.org/TR/microdata/), and[JSON-LD](https://www.w3.org/TR/json-ld11/).
Different search engines might adapt different vocabularies within schema.org, and no search engine covers all the use cases described the website's vocabulary. Be sure to check which vocabularies are accepted in each case.
An example of a what a product page might look like adding the JSON-LD product schema data:
```jsx
mport Head from 'next/head';

function ProductPage() {
  function addProductJsonLd() {
    return {
      __html: `{
      "@context": "https://schema.org/",
      "@type": "Product",
      "name": "Executive Anvil",
      "image": [
        "https://example.com/photos/1x1/photo.jpg",
        "https://example.com/photos/4x3/photo.jpg",
        "https://example.com/photos/16x9/photo.jpg"
       ],
      "description": "Sleeker than ACME's Classic Anvil, the Executive Anvil is perfect for the business traveler looking for something to drop from a height.",
      "sku": "0446310786",
      "mpn": "925872",
      "brand": {
        "@type": "Brand",
        "name": "ACME"
      },
      "review": {
        "@type": "Review",
        "reviewRating": {
          "@type": "Rating",
          "ratingValue": "4",
          "bestRating": "5"
        },
        "author": {
          "@type": "Person",
          "name": "Fred Benson"
        }
      },
      "aggregateRating": {
        "@type": "AggregateRating",
        "ratingValue": "4.4",
        "reviewCount": "89"
      },
      "offers": {
        "@type": "Offer",
        "url": "https://example.com/anvil",
        "priceCurrency": "USD",
        "price": "119.99",
        "priceValidUntil": "2020-11-20",
        "itemCondition": "https://schema.org/UsedCondition",
        "availability": "https://schema.org/InStock"
      }
    }
  `,
    };
  }
  return (
    <div>
      <Head>
        <title>My Product</title>
        <meta
          name="description"
          content="Super product with free shipping."
          key="desc"
        />
        <script
          type="application/ld+json"
          dangerouslySetInnerHTML={addProductJsonLd()}
          key="product-jsonld"
        />
      </Head>
      <h1>My Product</h1>
      <p>Super product for sale.</p>
    </div>
  );
}

export default ProductPage;
```
In this example, the data is hardcoded as a string, but you can easily pass the data to the `addProductJsonLd` method to make it fully dynamic.

# Sitemap Robots
下面是关于 nextjs 使用 [next-sitemap](https://www.npmjs.com/package/next-sitemap) 生成 sitemap.xml 和 robots.txt 的配置示例
```javascript
// https://www.npmjs.com/package/next-sitemap
// https://www.sitemaps.org/protocol.html

const exclude = ['/shopping-list', '/profile'];
/** @type {import('next-sitemap').IConfig} */
module.exports = {
  siteUrl: 'https://example.com',
  changefreq: 'monthly',
  priority: 0.7,
  sitemapSize: 5000,
  // This is useful for small/hobby sites which does not require an index sitemap
  generateIndexSitemap: false,
  // https://developers.google.com/search/docs/crawling-indexing/robots/intro?hl=zh-cn
  generateRobotsTxt: true,
  // /main-item: 使用 server side sitemap (getServerSideSitemap)
  exclude,
  // alternateRefs: [
  //   {
  //     href: 'https://es.example.com',
  //     hreflang: 'es',
  //   },
  //   {
  //     href: 'https://fr.example.com',
  //     hreflang: 'fr',
  //   },
  // ],
  // Default transformation function
  transform: async (config, path) => {
    console.log(path);
    
    return {
      loc: path, // => this will be exported as http(s)://<config.siteUrl>/<path>
      changefreq: config.changefreq,
      priority: path=== "/" ? 1 : config.priority,
      lastmod: config.autoLastmod ? new Date().toISOString() : undefined,
      alternateRefs: config.alternateRefs ?? [],
    }
  },
  // additionalPaths: async (config) => [
  //   await config.transform(config, '/additional-page'),
  // ],
  // https://developers.google.com/search/docs/crawling-indexing/robots/robots_txt?hl=zh-cn
  robotsTxtOptions: {
    policies: [
      {
        userAgent: '*',
        // allow: '/',
        disallow: exclude,
      },
    ],
    // additionalSitemaps: [
    //   'https://example.com/my-custom-sitemap-1.xml',
    //   'https://example.com/my-custom-sitemap-2.xml',
    //   'https://example.com/my-custom-sitemap-3.xml',
    // ],
  },
}
```


## sitemap.xml
放置网站根目录下，例如 https://treedeep.cn/sitemap.xml
https://www.sitemaps.org/protocol.html
[创建和提交站点地图](https://developers.google.com/search/docs/crawling-indexing/sitemaps/build-sitemap?hl=zh-cn)
[使用站点地图索引文件管理站点地图](https://developers.google.com/search/docs/crawling-indexing/sitemaps/large-sitemaps?hl=zh-cn)
上传 sitemap 有助于搜索引擎缩短爬虫发现网站链接的时间，加快收录网站的速度

- [谷歌站长工具](https://search.google.com/search-console)
- [百度站长工具](https://ziyuan.baidu.com/linksubmit/index)
- [必应站长工具](https://www.bing.com/webmasters/home)（可以关联谷歌站长工具）

如果网站有更新可使用 URL 提交 API 主动向搜索引擎站长工具推送（建议每天都提交一遍）

- 谷歌：[https://developers.google.com/webmaster-tools/v1/sitemaps/submit](https://developers.google.com/webmaster-tools/v1/sitemaps/submit)
- 百度：[https://ziyuan.baidu.com/linksubmit/index](https://ziyuan.baidu.com/linksubmit/index)
- 必应：[https://www.bing.com/webmasters/url-submission-api#APIs](https://www.bing.com/webmasters/url-submission-api#APIs)

百度不支持索引型 sitemap： [https://ziyuan.baidu.com/linksubmit/index](https://ziyuan.baidu.com/linksubmit/index)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1686047971507-edf448e0-4ce7-4950-8455-2bb889ea30bb.png#averageHue=%23fafafa&clientId=uc1e014eb-28bd-4&from=paste&height=485&id=cK7Og&originHeight=485&originWidth=956&originalType=binary&ratio=1&rotation=0&showTitle=false&size=69916&status=done&style=none&taskId=u4536b93f-9156-4f19-a903-36d0edc62ba&title=&width=956)

**示例**
```javascript
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://example.com/page-1</loc>
    <lastmod>2022-06-01T08:00:00+00:00</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.8</priority>
  </url>
  <url>
    <loc>https://example.com/page-2</loc>
    <lastmod>2022-06-03T10:00:00+00:00</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.7</priority>
  </url>
  <url>
    <loc>https://example.com/page-3</loc>
    <lastmod>2022-06-05T12:00:00+00:00</lastmod>
    <changefreq>daily</changefreq>
    <priority>0.5</priority>
  </url>
  <url>
    <loc>https://example.com/page-4</loc>
    <lastmod>2022-05-30T14:00:00+00:00</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.6</priority>
  </url>
</urlset>
```


## robots.txt
放置网站根目录下，例如 https://treedeep.cn/robots.txt
[robots.txt 规范](https://developers.google.com/search/docs/crawling-indexing/robots/robots_txt?hl=zh-cn)
[Robots meta 标记、data-nosnippet 和 X-Robots-Tag 规范](https://developers.google.com/search/docs/crawling-indexing/robots-meta-tag?hl=zh-cn)
```javascript
User-agent: *
Allow: /

Disallow: /api/*
```

# Google Analytics / baidu Analytics
[https://analytics.google.com/](https://analytics.google.com/)
[https://tongji.baidu.com/](https://tongji.baidu.com/)
```jsx
// lib/utils/analytics.ts
export type GTagEventOptions = {
    action: string;
    category: string;
    label: string;
    /**
     * @description A non-negative integer that will appear as the event value(价值，权重).
     */
    value?: number;
};

// https://developers.google.com/analytics/devguides/collection/gtagjs/pages
export const gTagPageView = (url: string) => {
    window.gtag("config", process.env.GA_ID, {
        page_path: url,
    });
};

// https://developers.google.com/analytics/devguides/collection/gtagjs/events
export const gtagEvent = ({ action, category, label, value }: GTagEventOptions) => {
    window.gtag("event", action, {
        event_category: category,
        event_label: label,
        value: value,
    });
};

// https://tongji.baidu.com/web/help/article?id=235
export const baiduPageView = (url: string) => {
    window._hmt.push(["_trackPageview", url]);
};
```

 参考：[https://github.com/vercel/next.js/blob/canary/examples/with-google-analytics/pages/_app.js](https://github.com/vercel/next.js/blob/canary/examples/with-google-analytics/pages/_app.js)
```jsx
// nextjs
<head>
  <script
    dangerouslySetInnerHTML={{
      __html: `
            window.dataLayer = window.dataLayer || [];
            function gtag(){dataLayer.push(arguments);}
            gtag('js', new Date());

            gtag('config', '${process.env.GA_ID}', {
                page_path: window.location.pathname,
            });
        `,
    }}
  />
  <script defer src={`https://www.googletagmanager.com/gtag/js?id=${GA_ID}`} />
  <script
    dangerouslySetInnerHTML={{
      __html: `
            var _hmt = _hmt || [];
            (function() {
              var hm = document.createElement("script");
              hm.defer=true;
              hm.src = "https://hm.baidu.com/hm.js?${process.env.BA_ID}";
              document.head.append(hm);
            })();
        `,
    }}
  />
</head>


import { useRouter } from "next/router";
import { baiduPageView, gTagPageView } from "@/lib/utils/analytics";

const router = useRouter();

useEffect(() => {
    // 上面的 script 标签会记录当前页面，对于 ajax 页面（跳转路由不会加载个页面），使用以下方法进行记录
    const handleRouteChange = (url) => {
      gTagPageView(url);
      baiduPageView(url);
    };

    router.events.on("routeChangeComplete", handleRouteChange);

    return () => {
      router.events.off("routeChangeComplete", handleRouteChange);
    };
  }, [router.events]);
```


# SEO Friendly URLs
[https://backlinko.com/hub/seo/urls](https://backlinko.com/hub/seo/urls)
[https://nextjs.org/learn/seo/rendering-and-ranking/url-structure](https://nextjs.org/learn/seo/rendering-and-ranking/url-structure)
URL Structure is an important part of an SEO strategy. While Google doesn't disclose which weight each part of SEO has, great URLs are considered a best practice no matter if they are a big or small ranking factor in the end.
You might want to follow some principles:

- **Semantic**: It's best to use URLs that are semantic, meaning that they use words instead of IDs or random numbers. Example: [/learn/basics/create-nextjs-app](https://nextjs.org/learn/basics/create-nextjs-app) is better than /learn/course-1/lesson-1
- **Patterns that are logical and consistent**: URLs should follow some sort of pattern that is consistent among pages. For example, you want to have a folder that groups all product pages, instead of having different paths for each product that you have.
- **Keyword focused**: Google still bases a considerable part of their systems on the keywords a website contains. You might want to use keywords in your URLs to facilitate understanding the purpose of the pages.
- **Not parameter-based**: Using parameters to build your URLs is generally not a good idea. They are not semantic in most cases, and search engines might confuse them and demote their rankings in results.
## How are Routes Defined in Next.js?
Next.js uses [file-system routing](https://nextjs.org/docs/routing/introduction) built on the concept of [pages](https://nextjs.org/docs/basic-features/pages). When a file is added to the pages directory, it is automatically available as a route. The files and folders inside the pagesdirectory can be used to define most common patterns.
Let's take a look at a couple of simple URLs and how you would add them to your Next.js router:

- **Homepage**: https://www.example.com → pages/index.js
- **Listings**: https://www.example.com/products → pages/products.js or pages/products/index.js
- **Detail**: https://www.example.com/products/product → pages/products/product.js

For a blog or e-commerce site you will likely want to use the product ID or blog name as the slug for the URL. This is called [dynamic routing](https://nextjs.org/docs/routing/dynamic-routes):

- **Product:**https://www.example.com/products/nextjs-shirt → pages/products/[product].js
- **Blog:**https://www.example.com/blog/seo-in-nextjs → pages/blog/[blog-name].js



