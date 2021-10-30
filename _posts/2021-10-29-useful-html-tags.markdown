---
layout: post
title: Useful HTML tags
date: 2021-10-29 12:39:00 +0530
categories: html
author: hardik
thumbnail: /assets/images/post/useful_html_tags.jpg
---

These tags are easy to use in developer life, but the developer doesn't know or not use them. so, In this article I'll describe some of the useful HTML tags. <!--more-->

## The HTML **```<base>```** Element

The ```<base>``` element specifies the base URL and/or target for all relative URLs in a page.<br/>
The ```<base>``` tag must have either an href or a target attribute present, or both.<br/>
There can only be one single ```<base>``` element in a document!

```html
<!DOCTYPE html>
<html>
<head>
  <base href="https://www.joghar.github.io/" target="_blank">
</head>
<body>
<h1>The base element</h1>
<p>
    <img src="images/dummy.jpg" width="24" height="39" alt="abc"> 
    - Notice that we have only specified a relative address for the image. 
    - Since we have specified a base URL in the head section, the browser will look for the image at "https://www.joghar.github.io/images/dummy.gif".
</p>

<p>
    <a href="blog/useful-html-tags">HTML base tag</a> 
    - Notice that the link opens in a new window, even if it has no target="_blank" attribute. 
    - This is because the target attribute of the base element is set to "_blank".</p>
</body>
</html>
```

## HTML **```<object>```** Tag

The ```<object>``` tag defines a container for an external resource.<br/>
The external resource can be a web page, a picture, a media player, or a plug-in application.<br/>

### Plug-ins
The ```<object>``` tag was originally designed to embed browser Plug-ins.<br/>
Plug-ins are computer programs that extend the standard functionality of the browser.<br/>
Plug-ins have been used for many different purposes:
- Run Java applets
- Run ActiveX controls
- Display Flash movies
- Display maps

``` html
<!DOCTYPE html>
<html>
<body>
<h1>The object element</h1>

<object data="snippet.html" width="500" height="200">
</object>
</body>
</html>
```

## HTML **```<meter>```** Tag

The ```<meter>``` tag defines a scalar measurement within a known range, or a fractional value. This is also known as a gauge.<br/>
Examples: Disk usage, the relevance of a query result, etc.<br/>
**Note:** The ```<meter>``` tag should not be used to indicate progress (as in a progress bar). For progress bars, use the ```<progress>``` tag.<br/>
**Tip:** Always add the ```<label>``` tag for best accessibility practices!

```html
<!DOCTYPE html>
<html>
<body>

<h1>The meter element</h1>
<p>The meter element is used to display a gauge:</p>

<label for="disk_c">Disk usage C:</label>
<meter id="disk_c" value="2" min="0" max="10">2 out of 10</meter><br>

<label for="disk_d">Disk usage D:</label>
<meter id="disk_d" value="0.6">60%</meter>
<p><strong>Note:</strong> The meter tag is not supported in Edge 12 (or earlier).</p>
</body>
</html>
```
output:<br/>
<label for="disk_d">Disk usage D:</label>
<meter id="disk_d" value="0.6">60%</meter>

## HTML **```<figure>```** and **```<figcaption>```** Elements
The ```<figure>``` tag specifies self-contained content, like illustrations, diagrams, photos, code listings, etc.<br/>
The ```<figcaption>``` tag defines a caption for a ```<figure>``` element. The ```<figcaption>``` element can be placed as the first or as the last child of a ```<figure>``` element.<br/>
The <img> element defines the actual image/illustration.

```html
<!DOCTYPE html>
<html>
<body>
<h2>Places to Visit</h2>

<figure>
  <img src="/assets/images/dummy.jpg" alt="dymmy" style="width:100%; height:200px">
  <figcaption class="text-center">Fig.1 - Dummy.</figcaption>
</figure>

</body>
</html>
```
output: 
<figure>
  <img src="/assets/images/dummy.jpg" alt="dymmy" style="width:100%; height:200px">
  <figcaption class="text-center">Fig.1 - Dummy.</figcaption>
</figure>

## HTML Computer Code Elements
### HTML ```<kbd>``` For Keyboard Input
The HTML ```<kbd>``` element is used to define keyboard input. The content inside is displayed in the browser's default monospace font.

```html
<!DOCTYPE html>
<html>
<body>
<h2>The kbd Element</h2>
<p>The kbd element is used to define keyboard input:</p>
<p>Save the document by pressing <kbd>Ctrl + S</kbd></p>
</body>
</html>
```
output: <kbd>Ctrl + S</kbd>

### HTML ```<samp>``` For Program Output
The HTML ```<samp>``` element is used to define sample output from a computer program. The content inside is displayed in the browser's default monospace font.

```html
<!DOCTYPE html>
<html>
<body>
<h2>The samp Element</h2>

<p>The samp element is used to define sample output from a computer program.</p>

<p>Message from my computer:</p>
<p><samp>File not found.<br>Press F1 to continue</samp></p>
</body>
</html>
```
output: <samp>File not found. Press F1 to continue</samp>

### HTML ```<code>``` For Computer Code
The HTML ```<code>``` element  is used to define a piece of computer code. The content inside is displayed in the browser's default monospace font.

```html
<!DOCTYPE html>
<html>
<body>
<p>The code element does not preserve whitespace and line-breaks.</p>

<p>To fix this, you can put the code element inside a pre element:</p>
<pre>
<code>
x = 5;
y = 6;
z = x + y;
</code>
</pre>
</body>
</html>
```
output:
<pre>
<code>
x = 5;
y = 6;
z = x + y;
</code>
</pre>

### HTML ```<var>``` For Variables
The HTML ```<var>``` element  is used to define a variable in programming or in a mathematical expression. The content inside is typically displayed in italic.

```html
<!DOCTYPE html>
<html>
<body>
<h2>The var Element</h2>
<p>The area of a triangle is: 1/2 x <var>b</var> x <var>h</var>, where <var>b</var> is the base, and <var>h</var> is the vertical height.</p>
</body>
</html>
```
output:
<p>The area of a triangle is: 1/2 x <var>b</var> x <var>h</var>, where <var>b</var> is the base, and <var>h</var> is the vertical height.</p>