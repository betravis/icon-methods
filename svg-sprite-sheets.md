---
layout: html
title: SVG Sprite Sheet Methods
---
[svg-fragment-ids]: http://www.w3.org/TR/SVG/linking.html#SVGFragmentIdentifiers
[svg-view]: http://www.w3.org/TR/SVG/linking.html#ViewElement
[can-i-use]: http://caniuse.com/#feat=svg-fragment
[wk-bug]: https://bugs.webkit.org/show_bug.cgi?id=137328
[icon-stacks]: http://simurai.com/blog/2012/04/02/svg-stacks/
[bashburn]: http://thenounproject.com/bashburn/
[original-bug]: https://bugs.webkit.org/show_bug.cgi?id=91790

# SVG Sprite Sheets

Turns out there are a couple nifty ways to use SVG as a sprite sheet, mentioned in SVG&rsquo;s [fragment identifier][svg-fragment-ids] section. You can use them anywhere you can use an image. Just drop them into an img tag or a CSS background.

## The Original SVG Sprite Sheet

This is the original SVG image, 300 units wide by 100 units tall.

<figure>
<img class='icon triple' src='resources/bunny-sprite-sheet.svg'>
</figure>

## With a Custom View Box

You can set a custom view box on an svg by adding a custom view box fragment identifier to the end of the SVG URL. It would look something like this:

<code>&lt;img&nbsp;src='bunnies.svg#svgView(viewBox(0,0,100,100))'&gt;</code>

<em>Note:</em> You will need to explicitly size the image, as some browsers (CR, S) will not clip to the custom view box.

<figure>
<img class='icon single' src='resources/bunny-sprite-sheet.svg#svgView(viewBox(0,0,100,100))'>
<img class='icon single' src='resources/bunny-sprite-sheet.svg#svgView(viewBox(100,0,100,100))'>
<img class='icon single' src='resources/bunny-sprite-sheet.svg#svgView(viewBox(200,0,100,100))'>
</figure>

## With a Custom View

You can also target a predefined [view element][svg-view] that encapsulates this information. In your SVG you would drop:

<code>&lt;view id='carrot-bunny' viewBox='0 0 100 100' /&gt;</code>

Which can then be used in HTML like so:

<code>&lt;img src='bunnies.svg#carrot-bunny'&gt;</code>

<figure>
<img class='icon single' src='resources/bunny-sprite-sheet.svg#carrot-bunny'>
<img class='icon single' src='resources/bunny-sprite-sheet.svg#happy-bunny'>
<img class='icon single' src='resources/bunny-sprite-sheet.svg#sad-bunny'>
</figure>

## Browser Support

{% include browser-support.html browser_support=site.data.support.custom_view_box %}

Browser support should roughly map to support for [SVG fragment identifiers][can-i-use]. FF, CR, O versions were only tested for the current release. IE 9 worked, though support is only listed on caniuse as 10 and up. Support for fragment ids is new for the Safari 7.1/iOS 8 release. It only works for image tags (not CSS backgrounds), and is pretty broken for this particular use case.

## Safari Issues
{: #safari-issues }
While the [original issue][wk-bug] in Safari 7.1 has been fixed as of Safari 9, the fix does not work for custom viewboxes on an svg as image. This may fix the original problem with [icon stacks][icon-stacks], though. There are a couple potential workarounds.

1. Revert to using the image as a CSS background and size/position it as a sprite sheet
2. Fragment identifiers specifying a viewBox will work with the `object` or `iframe` elements.
3. Fragment identifiers specifying a custom view element in the SVG will work with `iframe` elements.

So, your best bet is probably to use either `iframe` with a `view` element id:

    <iframe src='bunnies.svg#carrot-bunny'></iframe>

<figure>
<iframe class='icon single' src='resources/bunny-sprite-sheet.svg#carrot-bunny'></iframe>
<iframe class='icon single' src='resources/bunny-sprite-sheet.svg#happy-bunny'></iframe>
<iframe class='icon single' src='resources/bunny-sprite-sheet.svg#sad-bunny'></iframe>
</figure>

Or to use `object` with a custom `viewBox` fragment id:

    <object type='image/svg+xml' data='bunnies.svg?id=one#svgView(viewBox(0,0,100,100))'></object>
    <object type='image/svg+xml' data='bunnies.svg?id=two#svgView(viewBox(100,0,100,100))'></object>
    <object type='image/svg+xml' data='bunnies.svg?id=three#svgView(viewBox(200,0,100,100))'></object>

<figure>
<object class='icon single' type='image/svg+xml' data='resources/bunny-sprite-sheet.svg#svgView(viewBox(0,0,100,100))'></object>
<object class='icon single' type='image/svg+xml' data='resources/bunny-sprite-sheet.svg#svgView(viewBox(100,0,100,100))'></object>
<object class='icon single' type='image/svg+xml' data='resources/bunny-sprite-sheet.svg#svgView(viewBox(200,0,100,100))'></object>
</figure>

## Notes &amp; Credits

SVG sprite sheets are similar to [icon stacks][icon-stacks], and unfortunately suffers similar [issues][wk-bug] in Safari. What I like about this method is that all artwork is visible in the original image, and you don't have to do any style tinkering. You do have to do some offset math though, so you'll have to pick your poison here.

Icons are public domain from Brad Ashburn over on [the Noun Project][bashburn]. Inspired by the comments on a [WebKit bug][original-bug] about fragment identifier support.

<em>Update 10.23.15:</em> This article originally claimed full support in new versions of Safari. However, in most cases SVG sprite sheets will not work there without using a workaround. The article has been updated, and a full section added on [support in Safari](#safari-issues).

<em>Update 11.08.15:</em> With Safari 9, image elements basically ignore a custom `viewBox` on SVG. See [support in Safari](#safari-issues) for more info.
