# Creating pages

## File includes

Certain aspects and features of the page have already been created and are ready to be included. 

Here's the list of partial html files that you should include:
- `_head.html`: the `<head>` of the document.
- `_nav.html`: the navigation bar for the site.
-  `_join-modal.html`: the 'Join INFLOWSEM' modal which pops up after 40 seconds - should ideally be on every page. 
- `_cookie-alert.html`: alerts users to the use of cookies on the site.
- `_vendors.html`: adds the various vendors required for the site. Also includes a link to `main.js`.
- `_footer.html`: site footer.

 When including `_head.html`, make sure to pass the pass the title as an additional parameter. See below:
```html
  @@include('./partials/_head.html', {"title": "NEW_PAGE_TITLE})
```

## Navigation bar

It's important to remember that when you create a new page, you'll have to create a link to it in the navigation bar. 

Go to `partials/_nav.html`, and add the link. 

## Footer

Do the same for the footer. Go to `partials/_footer.html` and add a link to the page. 

## Sidebars

Some pages have their own navigation sidebars (industries and communities pages). If this is the case, e.g. you are adding a new industries page, then you need to add the sidebar to the page and add a link to it on all the other industries pages. 

Append the below code to the existing sidebars in the industries pages:
```html
<div class="navbar-side-item">
  <a class="navbar-side-link" href="NEW_PAGE_LINK.html">NEW_PAGE_NAME</a>
</div>
```

Then, copy the entire sidebar from one of the pages to the new page.