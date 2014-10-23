---
layout: html
title: SVG Sprite Sheet Methods
---
[svg-fragment-ids]: http://www.w3.org/TR/SVG/linking.html#SVGFragmentIdentifiers
[svg-view]: http://www.w3.org/TR/SVG/linking.html#ViewElement
[can-i-use]: http://caniuse.com/#feat=svg-fragment
[wk-bug]: https://bugs.webkit.org/show_bug.cgi?id=137900
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

Browser support should roughly map to support for [SVG fragment identifiers][can-i-use]. FF, CR, O versions were only tested for the current release. IE 9 worked, though support is only listed on caniuse as 10 and up. Support for fragment ids is new for the Safari 7.1/iOS 8 release, and there are still [some issues][wk-bug] that cause it to not work in most cases. Safari also only supports fragment ids on the image tag, not for CSS background images.

There is a hack to make this method work in Safari, but it&rsquo;s ugly and requires a duplicate image placed under the first.

<code>
&lt;img style='margin-right:-width' src='sheet.svg#viewId'&gt;
&lt;img src='sheet.svg#viewId'&gt;
</code>

<figure>
<img class='icon single hack' src='resources/bunny-sprite-sheet.svg#carrot-bunny'>
<img class='icon single' src='resources/bunny-sprite-sheet.svg#carrot-bunny'>

<img class='icon single hack' src='resources/bunny-sprite-sheet.svg#happy-bunny'>
<img class='icon single' src='resources/bunny-sprite-sheet.svg#happy-bunny'>

<img class='icon single hack' src='resources/bunny-sprite-sheet.svg#sad-bunny'>
<img class='icon single' src='resources/bunny-sprite-sheet.svg#sad-bunny'>
</figure>

## Notes &amp; Credits

SVG sprite sheets are similar to [icon stacks][icon-stacks]. What I like about this method is that <span class='strike'>it works in current versions of Safari</span>, and all artwork is visible in the original image. You do have to do some offset math though.

<em>Update 10.22:</em> There are many cases where this does not work in current versions of Safari. It looks similar to the [issue][wk-bug] with icon stacks, and I am investigating to see if there is a decent solution.

Icons are public domain from Brad Ashburn over on [the Noun Project][bashburn]. Inspired by the comments on a [WebKit bug][original-bug] about fragment identifier support.
