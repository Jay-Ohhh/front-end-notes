# SEO优化

https://developers.google.com/search/docs/beginner/seo-starter-guide?hl=zh-cn

https://nextjs.org/learn/seo/introduction-to-seo

# robots.txt

https://developers.google.com/search/docs/advanced/robots/create-robots-txt

# XML Sitemaps

https://developers.google.com/search/docs/advanced/sitemaps/overview

https://nextjs.org/learn/seo/crawling-and-indexing/xml-sitemaps



# [Open Graph protocol](https://ogp.me/)

https://nextjs.org/learn/seo/rendering-and-ranking/metadata

```js
import Head from 'next/head';

function IndexPage() {
  return (
    <div>
      <Head>
        <title>Cool Title</title>
        <meta name="description" content="Checkout our cool page" key="desc" />
        <meta property="og:title" content="Social Title for Cool Page" />
        <meta
          property="og:description"
          content="And a social description for our cool page"
        />
        <meta
          property="og:image"
          content="https://example.com/images/cool-page.jpg"
        />
      </Head>
      <h1>Cool Page</h1>
      <p>This is a cool page. It has lots of cool content!</p>
    </div>
  );
}

export default IndexPage;
```



#  Core Web Vitals

https://web.dev/vitals/
