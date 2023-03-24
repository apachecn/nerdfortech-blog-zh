# 如何在 Kivy 中创建后退按钮

> 原文：<https://medium.com/nerd-for-tech/how-to-create-a-back-button-in-kivy-3f22ba4840c4?source=collection_archive---------3----------------------->

我创建了一个 back 按钮，首先在我的 App 类中设置一个 __init__ 函数，然后使用 self.previous_screen 保存上一个屏幕的名称。我在。kv 文件，使其适用于所有屏幕。然后每当我创建后退按钮时，我都会引用 app.previous_screen。

main.py 文件:

```
from kivy.uix.screenmanager import Screen, ScreenManager
from kivy.app import App, Builder

Builder.load_file("medium.kv")

class Home(Screen):
    passclass User(Screen):
    pass

class RootWidget(ScreenManager):
    pass

class Test(App): def __init__(self, **kwargs):
        super(Test, self).__init__(**kwargs)
        self.previous_screen = "" 

    def build(self):
        return RootWidget()

if __name__ == '__main__':
    Test().run()
```

我使用了<screen>和 on_leave 属性来确保 self.previous_screen 在每次屏幕更改后都会更新。</screen>

```
<Screen>:
    on_leave: app.previous_screen = self.name

<Home>:
    name: "home_screen"
    GridLayout:
        cols: 1
        padding: 10
        Button:
            text: "To User Screen"
            on_release: root.manager.current = "user_screen"

<User>:
    name: "user_screen"
    GridLayout:
        cols: 1
        padding: 10
        Button:
            text: "Back"
            on_release: root.manager.current = app.previous_screen

<RootWidget>:
    Home
    User
```