---
layout: html
title: SVG Icon Methods
---
[bashburn]: http://thenounproject.com/bashburn/
[wk-stack-bug]: http://webkit.org/b/137900

<h1>SVG Icon Methods</h1>
<p>There are many different ways of packaging up vector assets in SVG. So many, in fact, that it&rsquo;s becoming a bit difficult to keep track of them all. I figured I would roll a bunch of the more popular ones up here as a handy reference.</p>

We will be using some lovely bunny artwork by [Brad Ashburn][bashburn].
<img src='bunny-sprite-sheet.svg' class='triple icon'>

<h2>Inline vs Linked SVG</h2>
One of the most direct ways of using SVG is to drop it right into your HTML document. This inline SVG format allows you to script and style between the HTML and SVG document.  When working with complex graphics, it is generally convenient to package them into external resources and link to them, as the SVG markup can get quite long. Most of these methods will focus on linked (external) icon packaging techniques.

### Inline SVG Example
<code>
&lt;!doctype html&gt;
&lt;html&gt;
&lt;svg&gt;&lt;path d="&hellip;" /&gt;&lt;/svg&gt;
&lt;/html&gt;
</code>

<img src="happy-bunny.svg"
<code>
&lt;img src="happy-bunny.svg"&gt;
</code>
{% include browser-support.html browser_support=site.data.support.basic %}

<h2>The Original SVG Sprite Sheet</h2>
<p>This is the original SVG image, 300 units wide by 100 units tall.</p>

<figure>
<img class='full' src='bunny-sprite-sheet.svg'>
</figure>

<h2>With a Custom View Box</h2>
<p>You can set a custom view box on an svg by adding a custom view box fragment identifier to the end of the SVG URL. It would look something like this:</p>
<p><code>&lt;img&nbsp;src='bunnies.svg#svgView(viewBox(0,0,100,100))'&gt;</code></p>
<p><em>Note:</em> You will need to explicitly size the image, as some browsers (CR, S) will not clip to the custom view box.</p>
<figure>
<img class='single' src='bunny-sprite-sheet.svg#svgView(viewBox(0,0,100,100))'>
<img class='single' src='bunny-sprite-sheet.svg#svgView(viewBox(100,0,100,100))'>
<img class='single' src='bunny-sprite-sheet.svg#svgView(viewBox(200,0,100,100))'>
</figure>

<h2>With a Custom View</h2>
<p>You can also target a predefined <a href='http://www.w3.org/TR/SVG/linking.html#ViewElement'>view element</a> that encapsulates this information. In your SVG you would drop:</p>
<p><code>&lt;view id='carrot-bunny' viewBox='0 0 100 100' /&gt;</code></p>
<p>Which can then be used in HTML like so:</p>
<p><code>&lt;img src='bunnies.svg#carrot-bunny'&gt;</code></p>

<figure>
<img class='single' src='bunny-sprite-sheet.svg#carrot-bunny'>
<img class='single' src='bunny-sprite-sheet.svg#happy-bunny'>
<img class='single' src='bunny-sprite-sheet.svg#sad-bunny'>
</figure>

<h2>Browser Support</h2>
<table class="support">
    <tr>
        <td><img src="browser-logos.svg#internet-explorer"></td>
        <td><img src="browser-logos.svg#firefox"></td>
        <td><img src="browser-logos.svg#chrome"></td>
        <td><img src="browser-logos.svg#safari"></td>
        <td><img src="browser-logos.svg#opera"></td>
        <td><img src="browser-logos.svg#ios"></td>
        <td><img src="browser-logos.svg#android"></td>
    </tr>
    <tr>
        <td class='support'>9</td>
        <td class='support'>32</td>
        <td class='support'>38</td>
        <td class='support'>7.1</td>
        <td class='support'>25</td>
        <td class='support'>8</td>
        <td class='no-support'>4.4</td>
    </tr>
</table>
<p>Browser support should roughly map to support for <a href="http://caniuse.com/#feat=svg-fragment">SVG fragment identifiers</a>. FF, CR, O versions were only tested for the current release. IE 9 worked, though support is only listed on caniuse as 10 and up. Support is new for the Safari 7.1/iOS 8 release.</p>

<h2>Notes &amp; Credits</h2>
<p>SVG sprite sheets are similar to <a href='http://simurai.com/blog/2012/04/02/svg-stacks/'>icon stacks</a>. What I like about this method is that it works in current versions of Safari (there is a <a href='http://webkit.org/b/137900'>bug</a> with icon stacks), and all artwork is visible in the original image. You do have to do some offset math though.</p>

<p>Icons are public domain from Brad Ashburn over on <a href='http://thenounproject.com/bashburn/'>the Noun Project</a>. Inspired by the comments on a <a href='https://bugs.webkit.org/show_bug.cgi?id=91790'>WebKit bug</a> about fragment identifier support.</p>
