# Adding news articles

Take the following code and paste it above the latest news article in `src/news.html`.

```html
<div class="news-article">
  <div class="container">
    <div class="row">
      <div class="article-body col-md-8">
        <div class="col-heading">...</div>
        <p>...</p>
      </div>
      <div class="article-sidebar col-md-4">
        <div class="news-article-img">
          <a href="...">
            <picture>
              <source srcset="..." type="image/webp">
              <source srcset="..." type="image/png">
              <img class="img-fluid news-article-img" src="..." alt="...">
            </picture>
          </a>
        </div>
      </div>
    </div>
  </div>
</div>
```

Note that `col-heading` is for the article heading and the `<p>` tags are for the article body.

There's also a `article-sidebar` which should be used to display the images in the article.

The `news-article-img` class in the anchor tag allows the image to be displayed in a lightbox, once clicked. This is initialised in `js/main.js` with the SimpleLightbox plugin. Wrapping an image in this tag, with the `news-article-image` class, will achieve this. 

Note the use of the `<picture>` tags. This allows for different file types. Chrome will default to the webp filetype (smaller than png or jpeg), improving performance. For more on this, see [HTML picture tag](https://www.w3schools.com/tags/tag_picture.asp).

