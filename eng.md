# Ten CSS One-Liners to Replace Native Apps

> Håkon Wium Lie is the father of CSS, the CTO of Opera, and a pioneer advocate 
for web standards. Earlier this year, we published his blog post, [“CSS Regions 
Considered Harmful.”][1] When Håkon speaks, whether we always agree or not, we 
listen. Today, Håkon introduces CSS Figures and argues their case.

Tablets and mobile devices require us to rethink web design. Moused scrollbars
will be replaced by paged gestures, and figures will float in multi-column
layouts. Can this be expressed in CSS?

Paged designs, floating figures, and multi-column layout are widely used on
mobile devices today. For some examples, see [Flipboard][2], the [Our Choice
ebook][3], or [Facebook Paper][4]. These are all native apps. If we want the web
to win on these devices (we do), it’s vital that designers can build these kinds
of presentations using web standards. If web standards cannot express this,
authors will be justified in making native apps.

Over the past years, I’ve been editing two specifications that, when combined,
provide this kind of functionality: [CSS Multi-column Layout][5] and [CSS
Figures][6]. I believe they are important to make sure the web remains a
compelling environment for content providers.

In this article, I will demonstrate how simple it is to write CSS code with
these specs. I will do so through 10 one-liners. Real stylesheets will be
slightly longer, but still compact, readable, and reusable. Here are some
screenshots to give you a visual indication of what we are aiming for:

![screenshot1][Three views of a web page demonstrating different numbers of columns for different window sizes]

## Building a page

The starting point for my code examples is an article with a title, text, and
some images. In a traditional browser, the article will be shown in one column,
with a scrollbar on the right. Using CSS Multi-column Layout, you can give the
article two columns instead of one:

    article { columns: 2 }

That’s a powerful one-liner, but we can do better; we can make the number of
columns depend on the available space, so that a narrow screen will have one
column, a wider screen will have two columns, etc. This is all it takes to
specify that the optimal line length is 15em and for the number of columns to be
calculated accordingly:

    article { columns: 15em }

To me, this is where CSS code morphs into poetry: one succinct line of code
scales from the narrowest phone to the widest TV, from the small print to text
for the visually impaired. There is no JavaScript, media queries, or expensive
authoring tool involved. There is simply one highly responsive line of code.
That line is used to great effect to produce the screenshots above. And it works
in current browsers (which is not yet the case for the following examples).

The screenshots above show paged presentations, as opposed to scrolled
presentations. This is easily expressed with:

    article { overflow: paged-x }

The above code says that the article should be laid out as pages, stacked along
the x-axis (which, in English, is toward the right). Browsers that support this
feature must provide an interface for navigating in these pages. For example,
the user may reach the next page by making a swiping gesture or tilting the
device. A visual indication of which page you are reading may also be provided,
just like scrollbars provide a visual indication in scrolled environments. On a
tablet or mobile phone, swiping to the next page or document will be easier than
scrolling.

## Images

Adding images to the article creates some challenges. Lines of text can easily
be poured into several columns, but if figures are interspersed with text, the
result will be uneven; because images are unbreakable, they will cause unused
whitespace if they appear at a column break. To avoid this, traditional paper-
based layouts place images at the top or bottom of columns, thereby allowing
other text to fill the whitespace. This can naturally be expressed in CSS by
adding `top` and `bottom` to the `float` property. For example:

    img { float: bottom }

The bluish harbor images in the screenshots above have been floated to the
bottom of the page with this one-liner. CSS is used to express something that
HTML cannot say; it is impossible to know how much textual content will fit on a
screen in advance of formatting. Therefore, an author cannot know where to
insert the image in the source code in order for it to appear at the bottom of
the column. Being able to float figures to the `top` and `bottom` (in addition
to the already existing `left` and `right`) is a natural extension to the
`float` property.

## Spanning columns

Another trick from traditional layout is for figures to span several columns.
Consider this newspaper clipping:

![screenshot2][A newspaper clipping showing text in four columns and images in the lower-left, lower-right and upper-right corners]

*Used with permission from the Bristol Observer*

In the newspaper article, the image on the left spans two columns and is floated
to the bottom of the columns. The code to achieve this in CSS is simple:

    figure { float: bottom; column-span: 2 }

HTML5’s `figure` element is perfect for holding both an image and the caption
underneath it:

    <figure>
        <img src=cats.jpg>
        <figcaption>Isabel loves the fluffy felines</figcaption>
    </figure>

The newspaper article also has a figure that spans three columns, and is floated
to the top right corner. In a previous version of the CSS Figures specification,
this was achieved by setting `float: top-corner`. However, after discussions
with implementers, it became clear that they were able to float content to more
places than just corners. Therefore, CSS Figures introduces new properties to
express that content should be [deferred][7] to a later column, page, or line.

## Deferring figures

To represent that the cat picture in the newspaper clipping should be placed at
the top of the last column, spanning three columns, this code can be used:

    figure { float: top; float-defer-column: last; column-span: 3 }

This code is slightly less intuitive (compared to the abandoned `top-corner`
keyword), but it opens up a range of options. For example, you can float an
element to the second column:

    .byline { float: top; float-defer-column: 1 }

The above code defers the byline, “By Collette Jackson”, by one. That is, if the
byline would naturally appear in the first column, it will instead appear in the
second column (as is the case in the newspaper clipping). For this to work with
HTML code, it is necessary for the byline to appear early in the article. For
example, like this:

    <article>
      <h1>New rescue center pampers Persians</h1>
      <p class=byline>By Collette Jackson</p>
      ...
    </article>

## Deferring ads

Advertisements are another type of content which is best declared early in the
source code and deferred for later presentation. Here’s some sample HTML code:

    <article>
      <aside id=ad1 src=ad1.png>
      <aside id=ad2 src=ad2.png>
      <h1>New rescue center pampers Persians</h1>
    </article>

And here is the corresponding CSS code, with a one-liner for each advertisement:

    #ad1 { float-defer-page: 1 }
    #ad2 { float-defer-page: 3 }

As a result of this code, the ads would appear on page two and four. Again, this
is impossible to achieve by placing ads inside the text flow, because page
breaks will appear in different places on different devices.

I think both readers and advertisers will like a more page-oriented web. In
paper magazines, ads rarely bother anyone. Likewise, I think ads will be less
intrusive in paged, rather than scrolled, media.

## Deferring pull quotes

The final example of content that can be deferred is pull quotes. A pull quote
is a quote lifted from the article, and presented in larger type at some
predetermined place on the page. In this example, the pull quote is shown midway
down the second column:

![screenshot3][A picture of a pull quote in a print newspaper]

Here’s the CSS code to express this in CSS:

    .pullquote#first { float-defer-line: 50% }

Other types of content can also be positioned by deferring lines. For example, a
photograph may be put above the fold of a newspaper by deferring a number of
lines. This will also work on the foldable screens of the future.

Pull quotes, however, are an interesting use case that deserve some discussion.
A pull quote has two functions. First, it presents to the reader an interesting
line of text to gain attention. Second, the presentation of the article becomes
visually more varied when the body text is broken up by the larger type.
Typically, you want one pull quote on every page. On paper, where you know how
many pages an article will take up, it is easy to supply the right number of
pull quotes. On the web, however, content will be reformatted for all sorts of
screens; some readers will see many small pages, other will see fewer larger
pages. To ensure that each page has a pull quote, authors must provide a
generous supply of pull quotes. Rather than showing the extraneous quotes at the
end of the article (which would be a web browser’s instinct), they should be
discarded; the content will anyway appear in the main article. This can be
achieved with a one-liner:

    .pullquote { float-policy: drop-tail }

In prose, the code reads: if the pull quote is at the tail-end of the article,
it should not be displayed. The same one-liner would be used to extraneous
images at the end of the article; authors will often want to have one image per
page, but not more than one.

## Exercises

The studious reader may want to consult the [CSS Multi-column Layout][8] and
[CSS Figures][9] specifications. They have more use cases and more knobs to
allow designers to describe the ideal presentation of figures on the web.

The easiest way to play with CSS Figures is to download [Opera 12.16][10] and
point it to [this document][11], which generated the screenshots in Figure 1.
Based on implementation experience, the specifications have changed and not all
one-liners presented in this article will work. Also, [Prince][12] and
[AntennaHouse][13] have partial support for CSS Figures—these are batch
formatters that output [PDF documents][14].

I’d love to hear from those who like the approach taken in this article, and
those who don’t. Do you want this added to browsers? Let me know below, or
request if from your favorite browser ([Firefox][15], [Chrome][16], [Opera][17],
[IE][18]). If you don’t like the features, how would you express the use cases
that have been discussed?

Pages and columns have been basic building blocks in typography since the Romans
started cutting scrolls into pages. This is not why browsers should support
them. We should do so because they help us make better, more beautiful, user
experiences on mobile devices.

[1]: http://alistapart.com/blog/post/css-regions-considered-harmful
[2]: https://flipboard.com/
[3]: http://www.ted.com/talks/mike_matas
[4]: https://www.facebook.com/paper
[5]: http://www.w3.org/TR/css3-multicol/
[6]: http://figures.spec.whatwg.org/
[7]: http://figures.spec.whatwg.org/#deferring-page-floats
[8]: http://www.w3.org/TR/css3-multicol/
[9]: http://figures.spec.whatwg.org/
[10]: http://www.opera.com/download/guide/?ver=12.16
[11]: http://www.wiumlie.no/2014/css-figures/koster/prefix.html
[12]: http://www.princexml.com/
[13]: http://www.antennahouse.com/
[14]: http://www.wiumlie.no/2014/css-figures/koster/prefix-pr.pdf
[15]: https://input.mozilla.org/en-US/feedback
[16]: https://productforums.google.com/forum/?rd=1#!categories/chrome/give-feature-feedback-and-suggestions
[17]: http://forums.opera.com/categories/en-opera-next-and-opera-developer
[18]: https://connect.microsoft.com/IE/feedback/

[Three views of a web page demonstrating different numbers of columns for different window sizes]: img/koster.jpg
[A newspaper clipping showing text in four columns and images in the lower-left, lower-right and upper-right corners]: img/newspaper.jpg
[A picture of a pull quote in a print newspaper]: img/pullquote.jpg