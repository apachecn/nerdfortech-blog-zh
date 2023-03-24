# 使用 python 抓取 Instagram

> 原文：<https://medium.com/nerd-for-tech/scrape-instagram-using-python-2576f09ebbb9?source=collection_archive---------2----------------------->

![](img/f0df2bf0b66ed3b7268f2fef8b69f63d.png)

这篇文章讲述了我们如何使用 python 连接到 Instagram，并提取关注者列表、你关注的人以及你不应该关注的人的列表(你关注但没有关注的人)😛)

我们可以使用一个名为`instaloader`的内置 python 包轻松完成以上工作。如果你想直接跳到代码，看看它的运行情况，这里是-[https://github.com/apoorva-dave/instagram-scraper](https://github.com/apoorva-dave/instagram-scraper)

# 属国

1.  Python3
2.  Instaloader
3.  Numpy

# 密码

创建一个处理以下 4 个步骤的文件`insta_scraper.py`

1.  创建会话

我们在下面的代码块中获得 Instaloader 的一个实例，并使用用户提供的用户名和密码登录。一旦完成，就创建了概要文件实例，以便获取概要文件的元数据。

```
def create_session(self): L = instaloader.Instaloader()
        L.login(self.username, self.password) # Login or load session
        self.profile = instaloader.Profile.from_username(L.context, self.username) # Obtain profile metadata
```

2.获取关注者列表

```
def scrape_followers(self): for follower in self.profile.get_followers():
            self.followers_list.append(follower.username)
```

3.获取以下列表

```
def scrape_following(self): for followee in self.profile.get_followees():
            self.following_list.append(followee.username)
```

4.获取取消关注列表

这将在您当前的目录中生成一个`unfollowers_<USERNAME>.txt`文件，其中包含您关注但他们不关注的人的列表。

```
def generate_unfollowers_list(self): unfollow_list = np.setdiff1d(self.following_list, self.followers_list) # unfollow people who are only in following list and not in followers list
        print("People to unfollow: ", unfollow_list)
        filename = "unfollowers_" + self.username + ".txt"
        file = open(filename, "w")
        for person in unfollow_list:
            file.write(person + "\n")
        file.close()
```

然后可以从 runner 脚本`main.py`中执行代码，该脚本将使用用户的用户名和密码调用`create_session()`。这样的设计是为了确保在创建会话 post 时只需要用户的用户名和密码，我们可以根据需要直接调用 API`scrape_followers()`等。

Instaloader 是一个非常高效的包。我们可以利用它做更多的事情。请查看这里的文档了解更多细节:[https://instaloader.github.io/as-module.html](https://instaloader.github.io/as-module.html)

你可以在这里找到完整的运行代码[和提供运行步骤的`README`。](https://github.com/apoorva-dave/instagram-scraper)

这就是这篇文章的内容。快乐学习！！❤