# Breakpoints Crashcourse
Places at which the CSS changes depending on the window size


## Setting the Viewport
This ensures that the native browser of the device is able to reflow content to match its screen dimensions 

    *Add this tag to the head of your HTML file*
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

## Some HTML tags react to screen dimensions differently
add this "catch-all" to prevent these items from overflowing the page even with the viewport fix

    img, embed, object, video {
        max-width: 100%;
    }

## Media Queries

### Minor Breakpoints

	@media screen and (min-width: 450px) and (max-width: 500px) {
        /* STYLING */
    }


## Flexbox Container
Allows reflowing of elements under resticted width dimensions

	.class {
		display: flex;
		flex-wrap: wrap;
		}

## Element Order

- Force element to be first with “order: -1;”
- Force element to be last with “order: 1;”
- The default order value is 0.

        @media screen and (min-width: 500px) 
        {
            .darkblue {
                width: 100%;
                order: 0;
            }
            .lightblue {
                width: 50%;
                order: 1;
            }
            .seablue {
                width: 50%;
                order: 2;
            }
        }

## Common Layout Patterns
- Mostly Fluid
    - Same as column drop but margins will form at final breakpoint
- Layout Shifter
    - Uses the “order: *” CSS attribute a lot → very responsive
- Column Drop
    - Elements are stacked vertically until breakpoints are hit, no margins
- Off Canvas
    - Places less frequent elements off-screen (i.e. hamburger menus, app menus, etc.)

CSS for "Off Canvas" Style:

    html, body, main {
        height: 100%;
        width: 100%;
    }
    /* default menu when off screen for mobile */
    .nav { 
        transform: translate(-300px, 0);
        transition: transform 0.3s ease;
    }
    /* toggles via clickEvent */
    nav.open {
        position: relative;
        transform: translate (0, 0);
        }
    /* menu auto-pops out at breakpoint for larger screens */
    @media screen and (min-width: 600px) {
        nav {
            position: relative;
            transform: translate (0, 0);
        }
        body {
            display: flex;
            flex-flow: row nowrap;
        }
        main {
            width: auto;
            flex-grow: 1;
        }
    }

## Responsive Table Styles (mobile-friendly)
Goal: To prevent ugly 'not optimized' tables on small screen especially due to the nature of their potrait screen configuration
1.  Hidden Columns
    - Uses breakpoints to prioritize only certain columns on smaller screens
    - At max-width media query → “display: none;”
2. No-more tables
    - Converts horizontal table with many columns, to a vertical list on small screens.

    Example:

        @media screen and (max-width: 500px) {
            table, thead, tbody, th, td, tr {
            display: block;
        }
        /* 
        hides table headers but still makes screen readers useable instead of “display: none;”
        */
        thead tr {
            position: absolute;
            top: -9999px;
            left:-9999px;
        }
        /* gives room for header */
        td {
            position: relative;
            padding-left: 50%;
        }
        /* adds row labels */
        td:before {
            position: absolute;
            left: 6px;
            content: attr(data-th);
            font-weight: bold;
        }

3. Contained Tables
    - Has the table overflow on the x-axis but can easily be scrolled on.

    Place table in a div, then:

        div.contained_table {
            width: 100%;
            overflow-x: auto;
        }

## General Line Characteristics
- Keep the “measure” of paragraphs in mind
    - The measure is the amount of words/characters per line.
    - Generally 65 characters per line is good practice for website design.
- Min font: 16px, 1.25em line height

Continue with: [Responsive Images](/image_optimization.md)




