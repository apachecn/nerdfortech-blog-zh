# 包含 HTML、CSS 和 JavaScript 的轮播滑块教程

> 原文：<https://medium.com/nerd-for-tech/carousel-slider-tutorial-with-html-css-and-javascript-373b6d308617?source=collection_archive---------13----------------------->

![](img/99ffa8b9c0922da3f8c4ca75284fd460.png)

在这篇文章中，我们将看看如何用 HTML、CSS 和 JavaScript 制作一个简单的旋转木马。我们将使用良好的代码实践，牢记可访问性，并考虑如何测试 carousel。

传送带将是一个“移动传送带”。幻灯片将从左到右或从右到左移动，并带有过渡。它不会是一个原地旋转，一张幻灯片淡出，而另一张幻灯片淡入。

如果你喜欢视频版本，这就是。它比这篇文章更详细。

# 基本功能

我们将从基本功能开始。这就是基本的 HTML、CSS 和 JavaScript。

# 超文本标记语言

我们将保持 HTML 相当简单。我们基本上需要:

*   旋转木马的容器
*   旋转式控件
*   幻灯片

除了旋转木马之外，我们不会过多关注 HTML 头或任何其他东西。其余的都是标准的东西。

至于实际的旋转木马，这里有一些我们可以使用的 HTML。

```
<head>
<!-- Import font-awesome somewhere in the HTML -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css" integrity="sha512-iBBXm8fW90+nuLcSKlbmrPcLa0OT92xO1BIsZ+ywDWZCvqsWgccV3gFoRBv0z+8dLJgyAHIhR35VZc2oM/gI1w==" crossorigin="anonymous" referrerpolicy="no-referrer" />
  <link rel="stylesheet" href="./index.css">
</head>

<body>
  <div class="carousel" data-carousel>
    <div class="carousel-buttons">
      <button
        class="carousel-button carousel-button_previous"
        data-carousel-button-previous
      >
        <span class="fas fa-chevron-circle-left"></span>
      </button>
      <button
        class="carousel-button carousel-button_next"
        data-carousel-button-next
      >
        <span class="fas fa-chevron-circle-right"></span>
      </button>
    </div>
    <div class="slides" data-carousel-slides-container>
      <div class="slide">
        <!-- Anything can be here. Each slide can have any content -->
        <h2>Slide 1 heading</h2>
        <p>Slide 1 content
      </div>
      <div class="slide">
        <!-- Anything can be here. Each slide can have any content -->
        <h2>Slide 2 heading</h2>
        <p>Slide 2 content
      </div>
    </div>
  </div>
</body>
```

在头部，我们链接了字体 awesome 和自定义样式 CSS 文件。

在正文中:

*   我们为整个传送带准备了一个外部`div`。
*   我们有两个按钮，一个用于“上一张幻灯片”，一个用于“下一张幻灯片”。按钮使用字体很棒的图标。
*   我们为幻灯片准备了一个`div`。在里面，我们为每张幻灯片都准备了一个`div`。每张幻灯片中的内容与我们无关，可以是任何内容。

至于`data-`属性，我们将在 JavaScript 中使用它们作为选择器。

我个人更喜欢对 JavaScript 使用`data-`属性，因为我想分离关注点。例如，类是 CSS 的标准用法。当将来有人试图改变 carousel 的样式时，他们可能会用一个更具描述性的名称来替换类名。他们可能还会改变一些 CSS 修饰符类什么的。我不希望他们偏执地认为，如果他们改变 CSS，他们可能会破坏 JavaScript，或者自动化测试，或者异步内容插入，或者其他任何东西。我希望他们在使用 CSS 时有安全感。

这意味着，在 JavaScript 中，我不使用类来选择元素。

一个例外是如果你使用带有前缀的类，比如`js-`。例如`<div class="js-carousel"></div>`，专门供 JavaScript 使用。那达到同样的结果。

但是我的偏好是使用`data-`属性。这就是`data-carousel`和其他人的目的。

# 半铸钢ˌ钢性铸铁(Cast Semi-Steel)

我们的 CSS:

1.  将会是我们旋转木马的基本造型
2.  将会有更换幻灯片的机制

我们的旋转木马的工作方式是让所有的幻灯片水平并排放置。但是，一次只能放映一张幻灯片。这是因为除了可见的那张幻灯片之外，每张幻灯片都将溢出顶层传送带`div`。那个`div`会有`overflow: hidden`，所以溢出的东西不会显示出来。

我们将用线`transform: translateX(/* something */)`决定当前正在播放哪张幻灯片。这样，我们将翻译`slides` div，以便只有正确的幻灯片是可见的。

这是 CSS。

```
.carousel {
  --current-slide: 0;
  /* we set position relative so absolute position works properly for the buttons */
  position: relative;
  overflow: hidden;
}

.carousel-button {
  /* vertically centering the buttons */
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  z-index: 1;

  /* basic styling */
  padding: 0;
  margin: 0.5rem;
  border-radius: 50%;
  background-color: transparent;
  border: none;

  font-size: 1.5rem;
  cursor: pointer;

  transition: color 0.1s;
}

.carousel-button:hover {
  color: rgba(0, 0, 0, 0.5);
}

.carousel-button_next {
  /* The "next slide button" will be at the right */
  right: 0;
}

.slides {
  display: flex;
  transition: transform 0.5s;
  transform: translateX(calc(-100% * var(--current-slide)));
}

.slide {
  flex: 0 0 100%;
}

@media screen and (min-width: 768px) {
  .carousel-button {
    font-size: 2rem;
    margin: 1rem;
  }
}
```

有了这个 CSS，每个`div`都有默认的 100%宽度。这意味着转盘将占据其父容器的整个宽度。每个载玻片也将占据转盘的整个宽度。

## 控制

在`carousel-button`类中，我们为按钮提供了一些简单的样式。我们使用字体很棒的图标，所以我们给它们一个字体大小，这样它们就大而可见。我们还移除了一些默认的按钮样式(比如边框和背景色)。

此外，我们将按钮放置在整个转盘的中间(垂直方向)。我们通过使用`position: absolute; top: 50%; transform: translateY(-50%);`技巧来做到这一点。

## 更换幻灯片

转盘如何改变幻灯片的诀窍在于`.slides`和`.slide`中的 CSS。在`.slide`中，我们让每张幻灯片的宽度为转盘宽度的 100%。这是通过`flex`属性完成的。换句话说，一个载玻片将占据转盘的整个宽度。

由于`.slides`是`display: flex;`，所有的载玻片将彼此水平相邻。这意味着一个载玻片将占据转盘的整个宽度，所有其他载玻片将在它旁边水平溢出。转盘 div 有`overflow: hidden;`，所以溢出的幻灯片都不会显示。

在某个时候，使用 JavaScript，我们将把`.slides` div 移到右边或左边。这意味着载玻片会移动，因此转盘内会出现不同的载玻片。

宣言`transform: translateX(calc(-100% * var(--current-slide)));`是我们的运动机制。这里我们说将幻灯片容器-100%(转盘的全宽，或幻灯片的全宽)向左移动(负号表示向左)，移动的次数与我们所在的幻灯片索引一样多。

例如，如果我们在幻灯片索引 0(第一张幻灯片)，`-100% * 0` = 0，那么我们根本不翻译，第一张幻灯片是可见的。

如果我们在幻灯片索引 1(第二张幻灯片)上，那么`-100% * 1` = -100%，所以我们向左平移 100%(一张幻灯片的宽度)。这意味着我们正在展示第二张幻灯片。

我们将使用 JavaScript 设置`--current-slide`属性。

# Java Script 语言

我们的 JavaScript 需要:

*   处理两个按钮的事件(切换到上一张幻灯片和下一张幻灯片)
*   为页面上任意数量的不同转盘独立工作

这是 JavaScript 代码。

```
function modulo(number, mod) {
  let result = number % mod;
  if (result < 0) {
    result += mod;
  }
  return result;
}

function setUpCarousel(carousel) {
  function handleNext() {
    currentSlide = modulo(currentSlide + 1, numSlides);
    changeSlide(currentSlide);
  }

  function handlePrevious() {
    currentSlide = modulo(currentSlide - 1, numSlides);
    changeSlide(currentSlide);
  }

  function changeSlide(slideNumber) {
    carousel.style.setProperty('--current-slide', slideNumber);
  }

  // get elements
  const buttonPrevious = carousel.querySelector('[data-carousel-button-previous]');
  const buttonNext = carousel.querySelector('[data-carousel-button-next]');
  const slidesContainer = carousel.querySelector('[data-carousel-slides-container]');

  // carousel state we need to remember
  let currentSlide = 0;
  const numSlides = slidesContainer.children.length;

  // set up events
  buttonPrevious.addEventListener('click', handlePrevious);
  buttonNext.addEventListener('click', handleNext);
}

const carousels = document.querySelectorAll('[data-carousel]');
carousels.forEach(setUpCarousel);
```

由于嵌套的函数，这段代码可能会显得有点混乱。如果你不习惯这种语法，那么这里有一个`setUpCarousel`函数的替代类，它做完全相同的事情。

```
class Carousel {
  constructor(carousel) {
    // find elements
    this.carousel = carousel;
    this.buttonPrevious = carousel.querySelector('[data-carousel-button-previous]');
    this.buttonNext = carousel.querySelector('[data-carousel-button-next]');
    this.slidesContainer = carousel.querySelector('[data-carousel-slides-container]');

    // state
    this.currentSlide = 0;
    this.numSlides = this.slidesContainer.children.length;

    // add events
    this.buttonPrevious.addEventListener('click', this.handlePrevious.bind(this));
    this.buttonNext.addEventListener('click', this.handleNext.bind(this));
  }

  handleNext() {
    this.currentSlide = modulo(this.currentSlide + 1, this.numSlides);
    this.carousel.style.setProperty('--current-slide', this.currentSlide);
  }

  handlePrevious() {
    this.currentSlide = modulo(this.currentSlide - 1, this.numSlides);
    this.carousel.style.setProperty('--current-slide', this.currentSlide);
  }
}

const carousels = document.querySelectorAll('[data-carousel]');
carousels.forEach(carousel => new Carousel(carousel));
```

基本上，我们持有一些状态，`currentSlide`和`numSlides`变量。我们还保存了对一些 HTML 元素的引用，比如 carousel 元素，因为我们在更换幻灯片时会用到它们。最后，我们向按钮添加事件侦听器。

当用户点击“下一张幻灯片”按钮时，我们运行`handleNext`功能。对`modulo(currentSlide, numSlides)`的调用将`currentSlide`设置为下一张幻灯片的正确索引。因此，如果有 5 张幻灯片，并且我们在幻灯片索引 0 上，它会将`currentSlide`设置为 1。但是，如果我们已经在幻灯片索引 4(第五张也是最后一张幻灯片)上，那么下一张幻灯片索引是 0，而不是 5。模函数为我们处理回 0 的包装。

实际上，我们可以使用`%`(模)操作符来实现这个目的。我们之所以有`modulo`函数，是因为`%`不擅长处理负数。`-1 % 5`评估为`-1`，而不是`4`(我们实际想要的幻灯片的索引)。我们创建了自己的`modulo`函数来处理这种情况。

最后，我们将 CSS 属性`--current-slide`设置为正确的数字。然后，CSS 通过适当地平移幻灯片`div`来改变可见的幻灯片。

页面上不同传送带的独立性是因为我们在父传送带元素上使用了`querySelector`，而不是在`document`上。这意味着，举例来说，`carouselElement1.querySelector([data-carousel-button-next])`，将只得到那个 carousel 元素里面的按钮。而`document.querySelector('[data-carousel-button-next]')`将获得它在页面上找到的第一个匹配元素，而不是目标轮播。

# 易接近

目前，这个转盘对屏幕阅读器用户非常不友好。您将需要实际使用屏幕阅读器并亲自聆听(或观看嵌入视频的可访问性部分)，但基本上:

*   它没有提到任何关于内容是一个传送带
*   对于按钮，它只显示“button”而没有其他内容(因为按钮没有文本或标签)
*   在“自动阅读”模式下，它会通读每张幻灯片的所有内容，就好像它是一个充满文本的普通网页一样(因为我们并没有告诉它只阅读可见的幻灯片)

要解决这些问题，我们需要查阅 [WAI-ARIA 创作实践文档](https://www.w3.org/TR/wai-aria-practices-1.1/)。有一个旋转木马区。我们只是去那里，并按照指示。其实也不是太难。它为我们提供了一步一步的指导。

最后，我们的 HTML 看起来像这样:

```
<div
  class="carousel"
  aria-role="group"
  aria-roledescription="carousel"
  aria-label="Student testimonials"
  data-carousel
>
  <div class="carousel-buttons">
    <button
      class="carousel-button carousel-button_previous"
      aria-label="Previous slide"
      data-carousel-button-previous
    >
      <span class="fas fa-chevron-circle-left"></span>
    </button>
    <button
      class="carousel-button carousel-button_next"
      aria-label="Next slide"
      data-carousel-button-next
    >
      <span class="fas fa-chevron-circle-right"></span>
    </button>
  </div>
  <div
    class="slides"
    aria-live="polite"
    data-carousel-slides-container
  >
    <div
      class="slide"
      aria-role="group"
      aria-roledescription="slide"
      aria-hidden="false"
      aria-labelledby="bob"
    >
      <h2 id="bob">Bob</h2>
    </div>

    <div
      class="slide"
      aria-role="group"
      aria-roledescription="slide"
      aria-hidden="true"
      aria-labelledby="alice"
    >
      <h2 id="alice">Alice</h2>
    </div>
  </div>
</div>
```

我们所做的事情的简要总结是:

*   我们给转盘`div`增加了`aria-role`、`aria-roledescription`和`aria-label`。现在，屏幕阅读器说类似“学生奖状转盘”的东西，立即表明这是一个转盘，以及它代表什么内容。
*   对于每个按钮，我们都添加了一个`aria-label`。现在屏幕阅读器显示类似“按钮上一张幻灯片”的内容，而不仅仅是“按钮”。(这里的另一种技术是添加“仅屏幕阅读器文本”。这是存在于 HTML 中的文本，但是使用特定的方法隐藏起来。)
*   我们给每张幻灯片添加了一个`aria-role`和`aria-roledescription`。现在屏幕阅读器知道它何时进入或离开幻灯片，并在必要时通知用户。
*   我们还使用`aria-labelledby`为每张幻灯片添加了一个标签。这与`aria-label`相同，只是您使用 HTML ID 将它指向页面上已经存在的一些文本。在这种情况下，由于我们的标签已经存在于页面上(每张幻灯片的标题)，我们使用了`aria-labelledby`而不是`aria-label`。
*   我们将`aria-hidden="true"`添加到隐藏的幻灯片中。现在屏幕阅读器不会读取它们。
*   我们增加了一个`aria-live`区域。现在，只要有更改(当用户更改幻灯片时)，屏幕阅读器就会重新读取转盘的内容。

还有其他一些有用的 aria 属性，但是我现在忽略它们，因为在 WAI-ARIA 创作实践的 carousel 部分没有提到它们。一个例子是 [aria 控件](https://tink.uk/using-the-aria-controls-attribute/)。如果你想了解更多，也许值得在业余时间看看 [WAI-ARIA 创作实践](https://www.w3.org/TR/wai-aria-practices-1.1/)。如果你想了解更多关于可访问性的知识，我已经在[网页可访问性——你需要知道的一切](https://programmingduck.com/articles/accessibility)中写了一个学习指南。

我们的 JavaScript 也需要一些更新。具体来说，当我们更改幻灯片时，我们需要为新的活动幻灯片将`aria-hidden`属性更改为`false`。我们还需要隐藏我们不再看的上一张幻灯片。

下面是一些我们可以使用的示例代码:

```
function changeSlide(slideNumber) {
  // change current slide visually
  carousel.style.setProperty('--current-slide', slideNumber);

  // handle screen reader accessibility
  // here we're getting the elements for the previous slide, current slide and next slide
  const previousSlideNumber = modulo(slideNumber - 1, numSlides);
  const nextSlideNumber = modulo(slideNumber + 1, numSlides);
  const previousSlide = slidesContainer.children[previousSlideNumber];
  const currentSlideElement = slidesContainer.children[slideNumber];
  const nextSlide = slidesContainer.children[nextSlideNumber];

  // here, we're hiding the previous and next slides and unhiding the current slide
  previousSlide.setAttribute('aria-hidden', true);
  nextSlide.setAttribute('aria-hidden', true);
  currentSlideElement.setAttribute('aria-hidden', false);
}
```

# 测试

有什么方法可以测试这样的东西？

简而言之，我会为它编写端到端的测试。我会犹豫是否要为它编写单元测试。

原因如下。

一个端到端的测试向你展示了整个事情正常工作。

根据您的测试框架，您可以做如下事情:

*   检查只有特定的`div`(幻灯片)在页面上可见，其他的不可见
*   按下下一个/上一个滑动按钮后，检查正确的`div`(滑动)是否可见
*   检查更换幻灯片的过渡是否正常工作

但是如果你进行单元测试，你只能检查你的 JavaScript 是否工作正常。

您可以做一个测试，设置一些 HTML，然后运行您的 JavaScript，最后检查得到的 HTML 是否是您所期望的。

或者你可以做一些类似于窥探你的 JavaScript 代码的事情，运行你的 JavaScript 并确保你的窥探被调用。

对于第一个单元测试示例(您检查最终的 HTML)，问题是，虽然您的测试可能通过了，但是您的传送带可能没有工作。例如，有人可能已经改变了 CSS 的工作方式。他们可能将属性`--current-slide`重命名为`--index`或其他名称。也许他们改变了整个 CSS 机制来改变幻灯片(例如，为了提高性能)。

在这种情况下，您的 JavaScript 将无错误地执行，测试也将通过，但是传送带不会工作。

测试不会提供你的代码工作的信心。

他们唯一会做的就是冻结你的 JavaScript 实现。在这种情况下，您已经在浏览器中手动检查了转盘。你认为“我可以看到它在工作，让我为它写一些单元测试，检查 JavaScript 是否在执行 X”。这是为了防止任何人在将来不小心修改 JavaScript。如果他们这样做，测试将会失败。

但是，这也使得有意的改变更加困难。现在，如果你想在未来改变实现，你需要改变你的 CSS，JavaScript 和你的 10 个测试。这是人们不喜欢单元测试的原因之一。它们使得对实现的修改更加困难(至少对于像这样的单元测试来说)。

因此，出于这些原因，我个人建议改为编写端到端测试。现在，如果你真的想防止 JavaScript 中的意外变化，这很好。你需要做你需要做的。由你来决定内心的平静是否值得这些负面影响，以及编写这些测试所花费的时间。

至于单元测试的另一个场景，你检查你的间谍是否被调用，我只是看不出这有什么好处。通过这些测试，您甚至不能测试您的 JavaScript 是否如您所想的那样运行。只要您调用相同的函数，您可以在将来中断 JavaScript 实现，您的测试仍然会通过。

但是，这些只是我对此事的想法。我愿意接受不同的意见。如果你认为我遗漏了什么，请在下面留下评论。

# 最终注释

原来如此。我希望这篇文章对你有用。

如果你想更全面地了解代码，[这里是代码库](https://github.com/Programming-Duck/carousel)。

请注意，这并不意味着生产就绪。代码可以清理的更多。它可能会变得更适合您的需要。等等。

这只是一个小教程，向你展示如何制作一个简单的旋转木马的总体思路。

如果您有任何反馈，任何遗漏或可以做得更好的地方，或其他任何东西，请在下面留下评论。

好的，非常感谢，下次见。

*原载于 2021 年 5 月 27 日 https://programmingduck.com*[](https://programmingduck.com/articles/javascript-carousel)**。**