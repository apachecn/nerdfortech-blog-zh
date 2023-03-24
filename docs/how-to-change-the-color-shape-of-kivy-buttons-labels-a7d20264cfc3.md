# 如何改变 Kivy 纽扣和标签的颜色/形状

> 原文：<https://medium.com/nerd-for-tech/how-to-change-the-color-shape-of-kivy-buttons-labels-a7d20264cfc3?source=collection_archive---------0----------------------->

关于这一点，我写了一些其他的帖子，但最终找到了实现这些格式更改的最佳方式:canvas.before。

这似乎适用于其他 Kivy 小部件，我尝试格式化 RecycleView。它有点工作，但我需要更多的时间来使它看起来抛光。试试其他的，然后告诉我！

此外，请确保查看。py 文件，它影响/改变背景。Kivy 的默认背景是黑色。

这都是工作代码，所以我建议将其复制到一个文件中，并尝试不同的东西来真正理解 canvas.before 是如何工作的！

```
from kivy.uix.screenmanager import Screen, ScreenManager
from kivy.app import App, Builder
from kivy.core.window import Window

Builder.load_file("medium.kv")# --- this changes the app's default background --- #
Window.clearcolor = (0, 0.6, 0.1, 1.0)Window.size = (600, 400)

class User(Screen):
    pass

class RootWidget(ScreenManager):
    pass

class Test(App):

    def build(self):
        return RootWidget()

if __name__ == '__main__':
    Test().run()
```

。kv file:注意，不管你把@Label、@Button、RootWidget 或任何其他类放在哪个顺序。kv 文件。我的位置是个人偏好。

```
<TestLabel@Label>:
    font_size: 20
    background_color: 1, 1, 1, 1
    background_normal: ""
    canvas.before:
        Color:
            rgba: 0, 1, 0, 0.5
        Line:    # --- adds a border --- #
            width: 2
            rectangle: self.x, self.y, self.width, self.height

<TestButton@Button>:
    font_size: 20
    background_color: 0, 0, 0, 0
    background_normal: ""
    canvas.before:
        Color:
            rgba: 0, 1, 0, 0.5
        RoundedRectangle:
            size: self.size
            pos: self.pos
            radius: [55]  #---- This rounds the corners --- #

<User>:
    name: "user_screen"
    GridLayout:
        padding: 15, 15
        cols: 1
        TestButton:
            text: "This is the button"
            size_hint: 0.3, 0.3
            color: 0, 0, 0, 1
        TestLabel:
            text: "This is the label"
            size_hint: 0.2, 0.2
        Button:
            text: "This is the default button"
            size_hint: 0.2, 0.2
        Label:
            text: "This is the default label"
            size_hint: 0.3, 0.3

<RootWidget>:
    User
```