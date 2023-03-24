# 基维:用。get_screen()从其他屏幕访问对象

> 原文：<https://medium.com/nerd-for-tech/kivy-use-get-screen-to-access-objects-from-other-screens-8d4d6f288f3?source=collection_archive---------0----------------------->

要访问不同屏幕中的对象，您只需要。get_screen()方法、屏幕名称、对象 id 和对象属性。例如，假设我想从不同的屏幕上引用主屏幕上的 TextInput。

从另一个屏幕快速参考主屏幕，主屏幕名称为 home_screen，主屏幕文本输入 id 为 text_input:

```
#.py file:# ---- One option ---- #self.manager.get_screen("home_screen").ids.text_input.text = "new text"# ---- Another option: ---- #reference_to_next_screen = self.manager.get_screen("home_screen")
reference_to_next_screen.ids.text_input.text = "new text"# .kv file:# replace self with root:root.manager.get_screen("home_screen).ids.text_input.text = "new"
```

对 OtherScreen 的引用在。kv 文件下面。py 文件:

```
from kivy.app import App
from kivy.lang import Builder
from kivy.uix.screenmanager import ScreenManager, Screen
from kivy.core.window import Window

Builder.load_file("medium.kv")

Window.clearcolor = (0, 0.6, 0.1, 1.0)
Window.size = (400, 600)

class HomeScreen(Screen):
    pass

class OtherScreen(Screen):
    pass

class RootWidget(ScreenManager):
    pass

class MainApp(App):

    def build(self):
        self.title = "My Gardens App"

        return RootWidget()

if __name__ == "__main__":
    MainApp().run()
```

。kv 文件:

```
<HomeScreen>:
    name: "home_screen"
    GridLayout:
        cols: 1
        Label:
            id: home_title
            text: "Home Screen"
            font_size: 30
        TextInput:
            id: your_message
            hint_text: "Type your message here."
            font_size: 25
        Button:
            text: "Go to OtherScreen"
            font_size: 25
            on_press: root.manager.current = "other_screen"

<OtherScreen>
    name: "other_screen"
    GridLayout:
        cols: 1
        Label:
            id: search_title
            text: "Title"
            font_size: 25
        Button:
            text: "Click Here to Change Above 'Title'\nto your_message from HomeScreen"
            font_size: 20 # ---- This is the reference --- #
            on_press: root.ids.search_title.text = root.manager.get_screen("home_screen").ids.your_message.text Button:
            text: "Go Home"
            on_press: root.manager.current = "home_screen"

<RootWidget>:
    HomeScreen:
        name: "home_screen"
    OtherScreen:
        name: "other_screen"
```

这和上面做的是一样的，但是从。py 文件:

```
from kivy.app import App
from kivy.lang import Builder
from kivy.uix.screenmanager import ScreenManager, Screen
from kivy.core.window import Window

Builder.load_file("medium.kv")

Window.clearcolor = (0, 0.6, 0.1, 1.0)
Window.size = (400, 600)

class HomeScreen(Screen):
    pass

class OtherScreen(Screen):

    def get_home_screen_text_input(self):
        ref_to_other_screen = self.manager.get_screen("home_screen")
        self.ids.other_title.text = ref_to_other_screen.ids.your_message.text

class RootWidget(ScreenManager):
    pass

class MainApp(App):

    def build(self):
        self.title = "My Gardens App"

        return RootWidget()

if __name__ == "__main__":
    MainApp().run()
```

。kv 文件。和第一个相比只有一个变化。kv 文件—将其他屏幕上的 on_press 替换为中的功能。py 代码。更改标记为:

```
<HomeScreen>:
    name: "home_screen"
    GridLayout:
        cols: 1
        Label:
            id: home_title
            text: "Home Screen"
            font_size: 30
        TextInput:
            id: your_message
            hint_text: "Type your message here."
            font_size: 25
        Button:
            text: "Go to OtherScreen"
            font_size: 25
            on_press: root.manager.current = "other_screen"

<OtherScreen>
    name: "other_screen"
    GridLayout:
        cols: 1
        Label:
            id: other_title
            text: "Title"
            font_size: 25
        Button:
            text: "Click Here to Change Above 'Title'\into your_message from HomeScreen"
            font_size: 20 # -------- only change from the first .kv file -------- #
            on_press: root.get_home_screen_text_input() Button:
            text: "Go Home"
            on_press: root.manager.current = "home_screen"

<RootWidget>:
    HomeScreen:
        name: "home_screen"
    OtherScreen:
        name: "other_screen"
```