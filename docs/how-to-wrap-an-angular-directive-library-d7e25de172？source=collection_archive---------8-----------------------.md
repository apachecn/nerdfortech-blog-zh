# 如何包装一个角度方向库？

> 原文：<https://medium.com/nerd-for-tech/how-to-wrap-an-angular-directive-library-d7e25de172?source=collection_archive---------8----------------------->

![](img/32be27317b7830b4b40369c5b256c732.png)

您被要求在工作中实现 Angular 应用程序的一个新功能。当你坐在办公桌前，伸手去拿键盘时，一个想法突然出现在你的脑海里:“我不能成为第一个不得不实现这种东西的人。我打赌有一个图书馆能满足我的需要”。

对你有好处。在今天的开源世界中，这是一个很好的条件反射。既然可以借用别人的轮子，为什么还要重新发明轮子呢？你很可能是对的；有人不得不解决你正在试图解决的同样的问题，并好心地与世界分享。

所以在 npmjs.com 上快速搜索一下，你就会找到你想要的东西。完美的 Angular 库，通过几个导出的指令，就可以完成你想要的功能。

现在，您意识到在整个应用程序中使用这些指令可能不是最好的主意，并且希望包装这个库，以便您的应用程序不会与它紧密耦合。但是怎么做呢？

当我们谈论包装第三方库时，我们通常谈论使用组合来为我们的应用程序提供新的接口，该接口将工作委托给第三方库。这样，第三方库就完成了所有繁重的工作，但我们的应用程序甚至不知道它的存在，它只知道我们为它制作的漂亮包装。

如果你熟悉设计模式，你可能最终会使用看起来很像[适配器](https://refactoring.guru/design-patterns/adapter)、[代理](https://refactoring.guru/design-patterns/proxy)或[外观](https://refactoring.guru/design-patterns/facade)模式的东西。

在我们的演示中，我们将包装[angular-resizable-element](https://github.com/mattlewis92/angular-resizable-element)库。您可以尝试一下，并在下面的 [Stackblitz](https://stackblitz.com/edit/wrapping-angular-directive?file=src/app/app.component.html) 中查看与本文相关的代码。

# 选择您的 API

是一个很酷的小库，可以通过拖动元素的边缘来调整元素的大小。让我们快速看一下它是如何工作的。根据它的[文档](https://mattlewis92.github.io/angular-resizable-element/docs/modules/ResizableModule.html)，它通过它的导出模块提供两个指令:`[ResizableDirective](https://mattlewis92.github.io/angular-resizable-element/docs/directives/ResizableDirective.html)`和`[ResizeHandleDirective](https://mattlewis92.github.io/angular-resizable-element/docs/directives/ResizeHandleDirective.html)`。

经过审查，我们得出结论，我们真的不需要使用`ResizeHandleDirective`。它的目的是对可调整大小的元素两侧的每个句柄进行更精细的控制，我们并不关心这个。所以我们只剩下`ResizableDirective`。查看文档，我们看到它接收 9 个输入，发出 3 个输出。

正如库经常出现的情况一样，它们提供的 API 往往比您实际需要的要广泛得多。不要觉得你必须用你的包装器来镜像第三方库。事实上，你的包装器的 API 应该只提供你的应用程序需要的东西。不多不少。

在我们的例子中，在仔细检查了我们的需求之后，我们确定我们不需要提供`allowNegativeResizes`、`mouseMoveThrottleMS`、`resizeCursors`、`resizeCursorPrecision`和`resizeSnapGrid`输入的等价物。除此之外，我们的包装器提供一个类似于第三方库的接口是有意义的，因为它将很好地满足我们的需求。

# 把它包起来

目前，我们的演示组件直接使用第三方库，代码如下所示:

```
<div class="text-center">
  <h1>Drag and pull the edges of the rectangle</h1>
  <div
    class="rectangle"
    [ngStyle]="style"
    mwlResizable
    [validateResize]="validate"
    [enableGhostResize]="true"
    (resizeEnd)="onResizeEnd($event)"
    [resizeEdges]="{bottom: true, right: true, top: true, left: true}"
  ></div>
</div>import { Component } from "@angular/core";
import { ResizeEvent } from "angular-resizable-element";

@Component({
  selector: "my-app",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  public style: object = {};

  validate(event: ResizeEvent): boolean {
    const MIN_DIMENSIONS_PX: number = 50;
    if (
      event.rectangle.width &&
      event.rectangle.height &&
      (event.rectangle.width < MIN_DIMENSIONS_PX ||
        event.rectangle.height < MIN_DIMENSIONS_PX)
    ) {
      return false;
    }
    return true;
  }

  onResizeEnd(event: ResizeEvent): void {
    this.style = {
      position: "fixed",
      left: `${event.rectangle.left}px`,
      top: `${event.rectangle.top}px`,
      width: `${event.rectangle.width}px`,
      height: `${event.rectangle.height}px`
    };
  }
}
```

如您所见，我们使用了模板库中的`mwlResizable`指令选择器及其组件中的`ResizeEvent`接口。我们需要使用我们的包装器。让我们开始吧。

## 第一步:输入和输出

作为第一步，我发现定义包装器的输入和输出非常有用。首先，我们将在新文件中为包装器创建一个新指令。由于我们计划提供一个类似的，但比库公开的接口更简单的接口，我们可以使用它的[源代码](https://github.com/mattlewis92/angular-resizable-element/blob/master/src/resizable.directive.ts)作为基础，简单地复制我们计划提供的输入和输出。完成这一步后，我们会得到如下结果:

```
@Directive({
  selector: "[resizableWrapper]"
})
export class ResizableDirective implements OnInit, OnChanges, OnDestroy {
  @Input() validateResize: (resizeEvent: ResizeEvent) => boolean;
  @Input() resizeEdges: Edges = {};
  @Input() enableGhostResize: boolean = false;
  @Input() ghostElementPositioning: "fixed" | "absolute" = "fixed";
  @Output() resizeStart = new EventEmitter<ResizeEvent>();
  @Output() resizing = new EventEmitter<ResizeEvent>();
  @Output() resizeEnd = new EventEmitter<ResizeEvent>();
}
```

您还需要确保不只是重用库的接口，而是提供自己的接口。例如，在上面的代码中，我们有`ResizeEvent`和`Edges`接口。我们确保在单独的文件中定义我们自己的。

## 第二步:构造函数参数

因为每当我们创建包装器的实例时，我们都将创建库指令的实例，所以我们将需要传递适当的依赖项。这里是第三方指令的构造函数:

```
constructor(
    @Inject(PLATFORM_ID) private platformId: any,
    private renderer: Renderer2,
    public elm: ElementRef,
    private zone: NgZone
  ) {
    this.pointerEventListeners = PointerEventListeners.getInstance(
      renderer,
      zone
    );
}
```

所以我们需要传递四个依赖项。这四个都是`@angular/core`包的一部分，因此 DI 系统应该很容易解决。让我们现在做那件事。

这一步不是特别难。我们所需要做的就是将库的指令添加到我们的包装器的构造函数中，并为 Angular 的 DI 提供一个工厂提供者。

```
function resizableDirectiveFactory(
  platformId: any,
  renderer: Renderer2,
  elm: ElementRef,
  zone: NgZone
) {
  return new ResizableDirective(platformId, renderer, elm, zone);
}

const resizableDirectiveProvider = { 
  provide: ResizableDirective,
  useFactory: resizableDirectiveFactory,
  deps: [PLATFORM_ID, Renderer2, ElementRef, NgZone]
};

@Directive({
  selector: "[resizableWrapper]",
  providers: [resizableDirectiveProvider]
})
export class ResizableWrapperDirective implements OnInit, OnChanges, OnDestroy {
  constructor(private library: ResizableDirective) {}
}
```

## 第三步:生命周期挂钩

用 Angular 包装指令时要记住的一点是，我们需要考虑生命周期挂钩。它们可以被视为包装器 API 的一部分。您可能希望拥有与您包装的指令相同的生命周期挂钩。记住这一点，让我们看看我们需要实现的三个挂钩。

先有`ngOnInit`。我们要做的第一件事是连接输出。

```
ngOnInit(): void {
  this.library.resizeStart
    .pipe(takeUntil(this.destroy$))
    .subscribe(event => this.resizeStart.emit(event));
  this.library.resizing
    .pipe(takeUntil(this.destroy$))
    .subscribe(event => this.resizing.emit(event));
  this.library.resizeEnd
    .pipe(takeUntil(this.destroy$))
    .subscribe(event => this.resizeEnd.emit(event));
}
```

记住，这个例子非常简单，因为我们的事件接口是库接口的镜像。如果不是这样，您就必须在发出它们之前将它们映射到您自己的接口。

好了，剩下的就是委托给图书馆自己的`ngOnInit`功能。

```
ngOnInit(): void {
  ...

  this.library.ngOnInit();
}
```

就这么简单。移动到`ngOnChanges`，它在`ngOnInit`之前被调用，并且每次一个或多个数据绑定输入属性改变时被调用。猜猜在这个函数中我们需要做什么。没错，分配我们的输入属性...并委托给库的`ngOnChanges`函数。

```
ngOnChanges(changes: SimpleChanges): void {
  if (changes.validateResize)
    this.library.validateResize = this.validateResize;
  if (changes.resizeEdges) this.library.resizeEdges = this.resizeEdges;
  if (changes.enableGhostResize)
    this.library.enableGhostResize = this.enableGhostResize;
  if (changes.ghostElementPositioning)
    this.library.ghostElementPositioning = this.ghostElementPositioning;

  this.library.ngOnChanges(changes);
}
```

最后，`ngOnDestroy`

```
ngOnDestroy(): void {
  this.library.ngOnDestroy();
  this.destroy$.next();
}
```

## 第四步:声明你的包装器并使用它

剩下的就是将我们的包装器添加到我们的模块中，并在我们的模板中使用它。

```
import { NgModule } from "@angular/core";
import { BrowserModule } from "@angular/platform-browser";
import { FormsModule } from "@angular/forms";

import { AppComponent } from "./app.component";
import { ResizableWrapperDirective } from "../lib/resizable-wrapper.directive";

@NgModule({
  imports: [BrowserModule, FormsModule],
  declarations: [AppComponent, ResizableWrapperDirective],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

如你所见，我们的模块没有引用第三方[角度可调整元素](https://github.com/mattlewis92/angular-resizable-element)库。它只声明了我们的包装指令。我们的模板和组件现在也只依赖于我们的包装指令。

```
<div class="text-center">
  <h1>Drag and pull the edges of the rectangle</h1>
  <div
    class="rectangle"
    [ngStyle]="style"
    resizableWrapper
    [validateResize]="validate"
    [enableGhostResize]="true"
    (resizeEnd)="onResizeEnd($event)"
    [resizeEdges]="{bottom: true, right: true, top: true, left: true}"
  ></div>
</div>
```

# 结论

包装第三方库通常是很好的实践，但是在处理角度指令时，这样做可能是一个挑战。每个库都是不同的，需要的方法也略有不同，但是本文中列出的四个步骤应该是一个很好的基础。