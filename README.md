## 网站性能优化项目

你要做的是尽可能优化这个在线项目的速度。注意，请应用你之前在[网站性能优化课程](https://cn.udacity.com/course/website-performance-optimization--ud884/)中学习的技术来优化关键渲染路径并使这个页面尽可能快的渲染。

开始前，请导出这个代码库并检查代码。

### 指南

####Part 1: 优化 index.html 的 PageSpeed Insights 得分

以下是几个帮助你顺利开始本项目的提示：

1. 将这个代码库导出
2. 你可以运行一个本地服务器，以便在你的手机上检查这个站点

```bash
  $> cd /你的工程目录
  $> python -m SimpleHTTPServer 8080
```

1. 打开浏览器，访问 localhost:8080
2. 下载 [ngrok](https://ngrok.com/) 并将其安装在你的工程根目录下，让你的本地服务器能够被远程访问。

``` bash
  $> cd /你的工程目录
  $> ./ngrok http 8080
```

1. 复制ngrok提供给你的公共URL，然后尝试通过PageSpeed Insights访问它吧！可选阅读：[更多关于整合ngrok、Grunt和PageSpeed的信息](http://www.jamescryer.com/2014/06/12/grunt-pagespeed-and-ngrok-locally-testing/)。

接下来，你可以一遍又一遍的进行配置、优化、检测了！祝你好运！

----

####Part 2: 优化 pizza.html 的 FPS（每秒帧数）

你需要编辑 views/js/main.js 来优化 views/pizza.html，直到这个网页的 FPS 达到或超过 60fps。你会在 main.js 中找到一些对此有帮助的注释。

你可以在 Chrome 开发者工具帮助中找到关于 FPS 计数器和 HUD 显示的有用信息。[Chrome 开发者工具帮助](https://developer.chrome.com/devtools/docs/tips-and-tricks).

### 一些关于优化的提示与诀窍
* [web 性能优化](https://developers.google.com/web/fundamentals/performance/ "web 性能")
* [分析关键渲染路径](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp.html "分析关键渲染路径")
* [优化关键渲染路径](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/optimizing-critical-rendering-path.html "优化关键渲染路径！")
* [避免 CSS 渲染阻塞](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css.html "css渲染阻塞")
* [优化 JavaScript](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/adding-interactivity-with-javascript.html "javascript")
* [通过 Navigation Timing 进行检测](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/measure-crp.html "nav timing api")。在前两个课程中我们没有学习 Navigation Timing API，但它对于自动分析页面性能是一个非常有用的工具。我强烈推荐你阅读它。
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/eliminate-downloads.html">下载量越少，性能越好</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html">减少文本的大小</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization.html">优化图片</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching.html">HTTP缓存</a>

### 使用 Bootstrap 并定制样式
这个项目基于 Twitter 旗下的 <a href="http://getbootstrap.com/">Bootstrap框架</a> 制作。所有的定制样式都在项目代码库的 `dist/css/portfolio.css` 中。

* <a href="http://getbootstrap.com/css/">Bootstrap CSS</a>
* <a href="http://getbootstrap.com/components/">Bootstrap组件</a>

## File Structure

###### public/

Contains the production ready CSS, JS and images built from the app/ directory.

###### views/

Contains the HTML for the pizza and individual project pages.

## The Build

The [Brunch](http://brunch.io) HTML5 build tool is used to concatenate and minify scripts and style sheets in this project.

###### Install Brunch

```
$ npm install -g brunch
```

###### Develop

```
$ brunch watch --server
```

###### Build

```
$ brunch build --production
```

Detailed documentation can be found [here](https://github.com/brunch/brunch/tree/stable/docs).

## Optimization

### Index Page

The index page originally had a Google PageSpeed score of 35/100 for mobile and 47/100 for desktop. After making changes the score increased to [99/100](https://developers.google.com/speed/pagespeed/insights/?url=http%3A%2F%2Foptimization.mikejoyce.io) for both mobile and desktop. Interestingly enough, the only thing that is preventing a score of 100/100 is Google's own analytics script.

The following changes were made:

###### - CSS

[Inlined](https://developers.google.com/speed/pagespeed/module/filter-css-inline) all of the CSS into the head of the document and added the HTML [media="print"](https://developer.mozilla.org/de/docs/Web/HTML/Element/link) attribute to the external style sheet link for print styles.

###### - JS

Added the [HTML async attribute](https://developer.mozilla.org/en-US/docs/Games/Techniques/Async_scripts) to all script tags and used the [Brunch](http://brunch.io/) build tool to concatenate and minify.

###### - Images

Resized images that were too large and compressed all images with the [Kraken](https://kraken.io/web-interface) image compression tool.

###### - Gzip compression

Enabled the [mod_deflate](http://httpd.apache.org/docs/2.2/mod/mod_deflate.html) (gzip) Apache module on the server.

###### - Browser Caching

Leveraged [browser caching](https://developers.google.com/speed/docs/insights/LeverageBrowserCaching) by including an [.htaccess](http://httpd.apache.org/docs/2.2/howto/htaccess.html) file in the root of the website. The file contains expires headers, which sets long expiration times for all CSS, JavaScript and images.

### Sliding Pizzas

The following changes where made to fix the low FPS and produce a consistent 60FPS frame rate when scrolling the page:

###### - Fixed Typo

Renamed the mis-named 'noise' value in the global adjectives array literal to ‘noisy’ to match the switch case ‘noisy’ in the getAdj function.

```js
var adjectives = ["dark", "color", "whimsical", "shiny", "noisy", "apocalyptic", "insulting", "praise", "scientific"];`
```

###### - Optimized Loops

Optimized the loops contained in the updatePositions function and the onDOMContentLoaded event handler.

```js
function updatePositions() {
  frame++;
  window.performance.mark("mark_start_frame");

  var items = document.querySelectorAll('.mover');
  for (var i = items.length; i--;) {
    var phase = Math.sin((document.body.scrollTop / 1250) + (i % 5));
    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
  }

  // User Timing API to the rescue again. Seriously, it's worth learning.
  // Super easy to create custom metrics.
  window.performance.mark("mark_end_frame");
  window.performance.measure("measure_frame_duration", "mark_start_frame", "mark_end_frame");
  if (frame % 10 === 0) {
    var timesToUpdatePosition = window.performance.getEntriesByName("measure_frame_duration");
    logAverageFrame(timesToUpdatePosition);
  }
}
```

```js
document.addEventListener('DOMContentLoaded', function() {
  var cols = 8;
  var s = 256;
  for (var i = 200; i--;) {
    var elem = document.createElement('img');
    elem.className = 'mover';
    elem.src = "../public/img/pizza.png";
    elem.style.height = "100px";
    elem.style.width = "73.333px";
    elem.basicLeft = (i % cols) * s;
    elem.style.top = (Math.floor(i / cols) * s) + 'px';
    document.querySelector("#movingPizzas1").appendChild(elem);
  }
  updatePositions();
});
```

###### - Reduced Pizza Elements

Reduced the amount of sliding pizza elements generated from 200 down to 31, which still sufficiently fills the screen with sliding pizzas.

```js
document.addEventListener('DOMContentLoaded', function() {
  var cols = 8;
  var s = 256;
  for (var i = 31; i--;) {
    var elem = document.createElement('img');
    elem.className = 'mover';
    elem.src = "../public/img/pizza.png";
    elem.style.height = "100px";
    elem.style.width = "73.333px";
    elem.basicLeft = (i % cols) * s;
    elem.style.top = (Math.floor(i / cols) * s) + 'px';
    document.querySelector("#movingPizzas1").appendChild(elem);
  }
  updatePositions();
});
```

###### - Improved CSS Animation Performance

Applied translateX() and translateZ(0) transform functions to the sliding pizza elements within the updatePositions function.


```js
function updatePositions() {
  frame++;
  window.performance.mark("mark_start_frame");

  var items = document.querySelectorAll('.mover');
  for (var i = items.length; i--;) {
    var phase = Math.sin((document.body.scrollTop / 1250) + (i % 5));
    //items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
    var left = -items[i].basicLeft + 1000 * phase + 'px';
 		items[i].style.transform = "translateX("+left+") translateZ(0)";
  }

  // User Timing API to the rescue again. Seriously, it's worth learning.
  // Super easy to create custom metrics.
  window.performance.mark("mark_end_frame");
  window.performance.measure("measure_frame_duration", "mark_start_frame", "mark_end_frame");
  if (frame % 10 === 0) {
    var timesToUpdatePosition = window.performance.getEntriesByName("measure_frame_duration");
    logAverageFrame(timesToUpdatePosition);
  }
}
```

###### - Improved Efficiency

Moved the calculation which utilizes the scrollTop method outside of the loop.

```js
function updatePositions() {
  frame++;
  window.performance.mark("mark_start_frame");

  var items = document.querySelectorAll('.mover');
  var top = (document.body.scrollTop / 1250);

  for (var i = items.length; i--;) {
    var phase = Math.sin( top + (i % 5));
    //items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
    var left = -items[i].basicLeft + 1000 * phase + 'px';
 		items[i].style.transform = "translateX("+left+") translateZ(0)";
  }

  // User Timing API to the rescue again. Seriously, it's worth learning.
  // Super easy to create custom metrics.
  window.performance.mark("mark_end_frame");
  window.performance.measure("measure_frame_duration", "mark_start_frame", "mark_end_frame");
  if (frame % 10 === 0) {
    var timesToUpdatePosition = window.performance.getEntriesByName("measure_frame_duration");
    logAverageFrame(timesToUpdatePosition);
  }
}
```

###### - Reduced Browser Paint Events

Removed height and width styles from the generated pizza elements and resized the pizza image to 100 x 100 to prevent the browser from having to resize the images.

```js
document.addEventListener('DOMContentLoaded', function() {
  var cols = 8;
  var s = 256;
  for (var i = 31; i--;) {
    var elem = document.createElement('img');
    elem.className = 'mover';
    elem.src = "../public/img/pizza-slider.png";
    elem.basicLeft = (i % cols) * s;
    elem.style.top = (Math.floor(i / cols) * s) + 'px';
    document.querySelector("#movingPizzas1").appendChild(elem);
  }
  updatePositions();
});
```

###### - Optimized Animations

Added the updatePositions function as a parameter to the window.requestAnimationFrame method in the scroll event listener which optimizes concurrent animations together into a single reflow and repaint cycle.

```js
window.addEventListener('scroll', function() {
	window.requestAnimationFrame(updatePositions);
});
```

### Resized Pizzas

The following changes were made to resize the pizzas in under 5ms:

###### - Improved Efficiency

Moved the determineDx function call inside the changePizzaSizes function out of the loop. Selected only the first .randomPizzaContainer in the document.

```js
function changePizzaSizes(size) {
	var dx = determineDx(document.querySelector(".randomPizzaContainer"), size);
  for (var i = 0; i < document.querySelectorAll(".randomPizzaContainer").length; i++) {
    var newwidth = (document.querySelectorAll(".randomPizzaContainer")[i].offsetWidth + dx) + 'px';
    document.querySelectorAll(".randomPizzaContainer")[i].style.width = newwidth;
  }
}
```

Moved the newwidth calculation inside the changePizzaSizes function out of the loop. Again, selected only the first .randomPizzaContainer element in the document.

```js
function changePizzaSizes(size) {
	var dx = determineDx(document.querySelector(".randomPizzaContainer"), size);
	var newwidth = (document.querySelector(".randomPizzaContainer").offsetWidth + dx) + 'px';
  for (var i = 0; i < document.querySelectorAll(".randomPizzaContainer").length; i++) {
    document.querySelectorAll(".randomPizzaContainer")[i].style.width = newwidth;
  }
}
```

Created a new variable to hold all of the .randomPizzaContainer elements in the document and looped through the elements to apply the new width value.

```js
function changePizzaSizes(size) {
	var dx = determineDx(document.querySelector(".randomPizzaContainer"), size);
	var newwidth = (document.querySelector(".randomPizzaContainer").offsetWidth + dx) + 'px';
	var elements = document.querySelectorAll(".randomPizzaContainer");
  for (var i = 0; i < elements.length; i++) {
    elements[i].style.width = newwidth;
  }
}
```

###### - Optimized Loops

Optimized loop inside the changePizzaSizes function.

```js
function changePizzaSizes(size) {
	var dx = determineDx(document.querySelector(".randomPizzaContainer"), size);
	var newwidth = (document.querySelector(".randomPizzaContainer").offsetWidth + dx) + 'px';
	var elements = document.querySelectorAll(".randomPizzaContainer");
  for (var i = elements.length; i--;) {
    elements[i].style.width = newwidth;
  }
}
```

## Optimization Breakdown (tl;dr)

### Index Page

###### Google PageSpeed Score after fixes

![Breakdown Image 12](readme_images/breakdown-02.png)

### Sliding Pizzas

###### FPS before any fixes

![Breakdown Image 01](readme_images/breakdown-03.png)

###### After fixing mis-named adjectives array literal value

![Breakdown Image 02](readme_images/breakdown-04.png)

###### After optimizing loops

![Breakdown Image 03](readme_images/breakdown-05.png)

###### After reducing the number of sliding pizzas generated from 200 to 31

![Breakdown Image 04](readme_images/breakdown-06.png)

###### After applying translateX() and translateZ(0) transform functions to the sliding pizza elements

![Breakdown Image 05](readme_images/breakdown-07.png)

###### After moving a calculation utlizing the scrollTop property outside of a loop

![Breakdown Image 06](readme_images/breakdown-08.png)

###### After removing height & width styles from pizza image tag and resizing the image

![Breakdown Image 07](readme_images/breakdown-09.png)

###### After including window.requestAnimationFrame method within scroll event handler

![Breakdown Image 08](readme_images/breakdown-10.png)
