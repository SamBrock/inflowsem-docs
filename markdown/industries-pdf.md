# Industries PDF files

## How it works
There is a pdf folder, within `src`, which contains all the individual pdf files. 

Also within `src/pdf`, there is a file called `pdf-viewer.html`. This html file displays the pdf as a series of slides, including next and prev buttons. All the css and JavaScript needed to make this work is also included in `pdf-viewer.html`, instead of the `src/js` and `src/css`.

To view the pdf in one of the industries pages, `pdf-view.html` is embeded into the page using the `<iframe>` tag.
```html
<iframe src="pdf/pdf-viewer.html?Optimal-sizing-and-life-cycle-cost-modelling-of-pipelines-carrying-solid-liquid-mixtures.pdf" class="pdf-iframe" scrolling="no" frameborder="0"></iframe>
```

As you can see from the above code, the actual pdf file is passed to `pdf-viewer.html` as a parameter in the query string of the src url. The code within `pdf-viewer.html` will then use this parameter to display the pdf file.

## Adding a new pdf
Use the following code when adding a new pdf file to a industries page:

```html
<section id="contribution-1">
  <div class="container text-center">
    <div class="row">
      <div class="col">
        <div class="col-heading">...</div>
        <p>...</p>
      </div>
    </div>
    <div class="row">
      <div class="col">
        <iframe src="pdf/pdf-viewer.html?<PDF_FILE>.pdf" class="pdf-iframe" scrolling="no" frameborder="0"></iframe>
      </div>
    </div>
  </div>
</section>
```
Replace `<PDF_FILE>` with the name of the pdf you have added to `src/pdf`. Note that the pdf title can't have any spaces in it. 

You should change `id="contribution-1"` if there is more than one pdf file already on the page (e.g. if there is 3, change the 1 to 4).

You may want to delete the `<p>` tags if there is no subheading. 

Once the pdf is added, it's important that you add a link to it in the industries sidebar. The following code is from `valves.html`:
```html
<div class="navbar-side-item active">
  <a class="navbar-side-link" href="valves.html">Valves</a>
  <ul class="nav bd-sidenav">
    <li class="navbar-side-sub-link"><a href="valves.html#contribution-1">Severe Service Control Valve Design Using CFD</a></li>
    <li class="navbar-side-sub-link"><a href="valves.html#contribution-2">Valve Flow Trim Optimization</a></li>
    <li class="navbar-side-sub-link"><a href="valves.html#contribution-3">Improved Design of Multistage Valve Trims</a></li>
    <li class="navbar-side-sub-link"><a href="valves.html#contribution-4">Improving Sustainability of the NPD Process Using CFD</a></li>
  </ul>
</div>
```
Here you can see a list of links to the different pdf files on the valves industries page. `<a href="valves.html#contribution-...">` is there so that users can click these links and be taken straight to the file, without having to scroll for it. 

There isn't a industries page sidebar file - instead, each industries page file has it's version with links to the pdf files within it.