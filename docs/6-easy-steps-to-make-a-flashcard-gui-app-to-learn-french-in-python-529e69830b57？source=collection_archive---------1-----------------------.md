# 用 Python 制作学习法语的抽认卡 GUI 应用程序的 6 个简单步骤

> 原文：<https://medium.com/nerd-for-tech/6-easy-steps-to-make-a-flashcard-gui-app-to-learn-french-in-python-529e69830b57?source=collection_archive---------1----------------------->

我们都经历过。学习一门外语可能是一项令人生畏的任务。谢天谢地，我们有 python 来拯救我们。我创建了一个抽认卡图形用户界面应用程序，它有助于记住难记的单词，并且可以被引用来更长时间地记住它们。余博士的另一个伟大的项目想法。

![](img/b80d09d92bb059eb77c94ef498ec04d5.png)

在 UI 设置中，一个法语单词闪现在卡片上，然后用户可以看着这个法语单词三秒钟，然后卡片翻转，包含相应的英语单词。

抽认卡是通过练习信息检索来测试和提高记忆力的好工具。也是那些想升级/增强词汇量或学习一门新语言的人最常用的工具。

# 构建抽认卡图形用户界面的步骤

1.  导入 pandas、random 和 Tkinter 库
2.  导入法语到英语单词数据库
3.  定义全局常数
4.  设置用户界面
5.  框住下一张牌并翻转牌逻辑
6.  生成法语单词和异常处理

# 步骤 1:导入熊猫，随机和数学库

```
import pandas
BACKGROUND_COLOR = "#B1B2DD" #To set the background colour 
import random
from tkinter import *
import pandas as pd
```

# 步骤 2:将法语单词导入英语单词数据库

该数据库可以在网上找到，不过我已经把它包括在我的回购。这里可以找到[。](https://github.com/Ayu-dxt777/100daysofpython/blob/main/Flashcard%20App/french_words.csv)

# 步骤 3:定义全局常数

```
to_learn = {}
current_card = {}
```

我们定义了两个全局常量“to_learn ”,它将包含法语单词数据库。

另一个是 current_card，它在抽认卡 GUI 中包含当前的卡。

# 步骤 4:设置用户界面

```
window = Tk()
window.title("Flashcard App")
window.config(padx=50, pady=50, bg=BACKGROUND_COLOR)
flip_timer = window.after(3000, func=flip_card) # 3000 milliseocnds = 3 seconds

canvas = Canvas(width=800, height=526)
card_front_img = PhotoImage(file="./images/card_front.png")
card_back_img = PhotoImage(file="./images/card_back.png")
card_background = canvas.create_image(400, 263, image=card_front_img)
card_title = canvas.create_text(400, 150, text="Title", font=("Ariel", 40, "italic"))
# Positions are related to canvas so 400 will be halfway in width
canvas.config(bg=BACKGROUND_COLOR, highlightthickness=0)
card_word = canvas.create_text(400, 263, text="Word", font=("Ariel", 60, "bold"), tags="word")
# canvas should go in the middle
canvas.grid(row=0, column=0, columnspan=2)

cross_image = PhotoImage(file="./images/wrong.png")
unknown_button = Button(image=cross_image, command = next_card)
unknown_button.grid(row=1, column=0, sticky="W")

check_image = PhotoImage(file="./images/right.png")
known_button = Button(image=check_image, command=is_known)
known_button.grid(row=1, column=1, sticky="E")

next_card()
window.mainloop()
```

UI 设置已经包含在我的另一个 pomodoro GUI 应用程序项目中。你能在这里找到什么。用户可以改变画布的尺寸和颜色，或者包括正确和错误标志的其他图像。当用户读完单词或者已经知道法语单词的英语翻译时，他可以点击正确或错误按钮。

我们将在后面定义两个函数“next_card”和“is_known”。

# 步骤 5:框住下一张卡并翻转卡逻辑

```
def next_card():
    global current_card, flip_timer
    window.after_cancel(flip_timer)
    current_card = random.choice(to_learn)
    # french_word = random_pair['French']
    canvas.itemconfig(card_title, text="French", fill="black")
    canvas.itemconfig(card_word, text=current_card["French"], fill="black")
    canvas.itemconfig(card_background, image=card_front_img)
    flip_timer = window.after(3000, func=flip_card)

def flip_card():
    canvas.itemconfig(card_title, text = "English", fill = "white")
    canvas.itemconfig(card_word, text=current_card["English"], fill = "white")
    canvas.itemconfig(card_background, image=card_back_img)

def is_known():
    to_learn.remove(current_card)
    print(len(to_learn))
    data = pd.DataFrame(to_learn)
    data.to_csv("data/words_to_learn.csv", index=False)
    # index = false discrads the index numbers
    next_card()
```

我们定义了三个函数 next_card()，flip_card()和 is_known()。当用户点击按钮时，调用 next_card。

我们定义了翻转定时器，它决定了翻转抽认卡的时间，还定义了 after_cancel 方法，它采用了 flip_card 函数并设置了 3 秒(3000 毫秒)的定时器。

flip_card()方法将卡片翻转为法语单词的英语翻译。请注意，填充颜色已经更改，以标记两张卡之间的差异。

最后，我们有 is_known()函数，它删除用户已经学习过的卡，并创建一个学习过的卡的数据库。(words_to_learn.csv)

## 使用 iterrows 的替代方法

```
data = pd.read_csv("./data/french_words.csv")
word_dict = {row.French:row.English for (index, row) in df.iterrows()}
#word_dict = df.to_dict(orient="records")
print(word_dict)
```

# 步骤 6:生成一个法语单词和异常处理

```
try: # try running this line of code
    data = pandas.read_csv("data/words_to_learn.csv")
except FileNotFoundError:
    # If for the first time we are running it
    # the words_to_learn.csv file might not be present
    # and FileNotFoundError might pop up
    original_data = pandas.read_csv("data/french_words.csv")
    print(original_data)
    to_learn = original_data.to_dict(orient="records")
else:
    to_learn = data.to_dict(orient="records")
# data = pd.read_csv("./data/words_to_learn.csv")
# word_dict = {row.French:row.English for (index, row) in df.iterrows()}
# spits out a list of dictionaries containing french word and english translation
# print(word_dict)
```

因为，我们正在对 words_to_learn.csv 进行更改。在第一次运行时，该文件不存在，所以我们通过首先尝试读取 words_to_learn.csv 来捕获此错误。

```
try: # try running this line of code
    data = pandas.read_csv("data/words_to_learn.csv")
```

如果在第一次运行中出现 FileNotFoundError 异常，我们将读取原始数据 french_words.csv。

此外，不要忘记在代码末尾给出，以阻止画布消失。

mainloop()告诉 Python 运行 Tkinter 事件循环。该方法侦听事件，如按钮点击或按键，并阻止在它之后运行的任何代码，直到调用它的窗口关闭。继续并关闭您创建的窗口，您将看到 shell 中显示一个新的提示。：

```
window.mainloop()
```

你可以在这里找到完整的代码。

你喜欢我的努力吗？如果是的话，请跟我来获取我的最新帖子和更新，或者最好请我喝杯咖啡！

[![](img/3a3be58c27c061a0f4057c746602e8e2.png)](https://www.buymeacoffee.com/ayushdixit)[](https://www.buymeacoffee.com/ayushdixit) [## ayushdixit 正在编码、部署项目和写博客

### 嘿👋我刚刚在这里创建了一个页面。你现在可以给我买杯咖啡了！

www.buymeacoffee.com](https://www.buymeacoffee.com/ayushdixit)