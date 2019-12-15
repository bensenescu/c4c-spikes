## Image Responsiveness: 
How to reduce file size without sacrificing quality



Abuse max-width: 100%;
	
    /* Calculate the width to fit a certain number of images (3) at a given margin in between (20px) */
    img {
        float: left;
        margin-right: 20px;
        width: calc((100% - 20px)/3);
    }

Vector images >>> Raster images for scaling

Other image-type articles:

- [Photo with logo as JPEG](http://udacity.github.io/responsive-images/examples/1-15/kittensPlusHtml5Logo)

- [Photo as JPEG, logo overlaid as SVG](http://udacity.github.io/responsive-images/examples/1-15/kittensPlusHtml5LogoSvg)

- [SVG v PNG v JPG](http://udacity.github.io/responsive-images/examples/1-15/svgPngJpg)

Image Compression Tools:

- Grunt -> Preferred
- Image magik
- Image Optim

Verify with the network developer setting in Chrome Dev Tools: the goal is to reduce the size of image files as much as possibly without sacrificing quality

## Use images that aren't images:
- Unicode characters should replace images when possible 
    - Zocial is a great font family for modern symbols
- Why? Reduces HTTP Requests
    - [SVG Data URI in HTML](http://udacity.github.io/responsive-images/examples/2-11/svgDataUri)
    - [SVG Data URI in CSS](http://udacity.github.io/responsive-images/examples/2-11/svgDataUriCss)
    - [SVG text on a path](http://udacity.github.io/responsive-images/examples/2-11/svgTextOnAPath)
    - [SVG optimized and unoptimized](http://udacity.github.io/responsive-images/examples/2-11/svgUnoptimisedAndOptimised)

## Lesser known CSS units for images in particular:

- height: 100vh;
    - This means that 100% of the viewport height
- Vmax, vmin, or a combination of the two can used for great image responsiveness 

## Use HTML 5 picture/figure tags for better predicatbility and performance.

    <figure>
        <img src="" alt="Img Title">
        <figcaption>Caption</figcaption>
    </figure>

### Backgroundsize: contain vs cover?


To be continued: