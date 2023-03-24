# 重构非开发人员代码

> 原文：<https://medium.com/nerd-for-tech/refactor-non-developer-codes-b29083281622?source=collection_archive---------12----------------------->

基于真实故事

![](img/7d1ee9d3b4bb7ed8550d8e6a43830be1.png)

这张图片改编自文件:2021 年 3 月 24 日，在泰国曼谷举行的一次集会上，亲民主抗议者在地板上画了一个标志，要求释放被捕的抗议领袖，并废除 112 lese majeste 法律。照片:路透社/豪尔赫·席尔瓦

我有机会为一个内部活动创建一个网站。我们的团队决定，这个网站的体验可以类似于设计团队创建的体验——他们最近刚刚举行了类似的活动——这种体验看起来整洁干净。

由于努力和时间有限(…和懒惰)，我决定使用他们的代码作为起点。

在与代码一起工作了一小段时间后，我看到了可以改进的模式和实践，例如命令式编程、死代码、未使用的依赖，并有了将它们写下来的想法。

## **免责声明**

我的目的不是批评任何人，而是记录学到的教训，这对以后的任何人都是有益的。

# 命令式编程

## 不使用数组方法—映射

这只是将`this.programsData`转换为`Session`对象的数组。在滚动、滚动…和滚动之后，很容易忘记这个操作的结果是什么。

```
let sessions = [];
let entries = this.programsData;for (var j = 0; j < entries.length; j++) {
  var newSession = new Session();
  var entry = entries[j]; var ID = entry.RowKey;
  if (ID) {
    newSession.ID = ID;
  } var title = entry.Title;
  if (title) {
    newSession.Title = title;
  } var track = entry.Track;
  if (track) {
    newSession.Track = entry.Track;
  } var type = entry.Type;
  if (type) {
    newSession.Type = type;
  } var description = entry.Description;
  if (description) {
    newSession.Description = description;
  } var language = entry.Language;
  if (language) {
    newSession.Language = language;
  } var calendarLink = entry.CalendarLink;
  if (calendarLink) {
    newSession.CalendarLink = calendarLink;
  } var eventX = entry.EventX;
  if (eventX) {
    newSession.EventX = eventX;
  } var eventXText = entry.EventXText;
  if (eventXText) {
    newSession.EventXText = eventXText;
  } else {
    newSession.EventXText = "Register in EventX";
  } var special = entry.Special;
  if (special) {
    newSession.Special = special;
  } var specialText = entry.SpecialText;
  if (specialText) {
    newSession.SpecialText = specialText;
  } var specialIcon = entry.SpecialIcon;
  if (specialIcon) {
    newSession.SpecialIcon = "images/em-icons.svg#" + specialIcon;
  } if (special && specialText) {
    if (!specialIcon) {
      newSession.SpecialIcon = "images/em-icons.svg#" + "exclamation-point";
    }
    newSession.ShowSpecial = true;
  } else {
    newSession.ShowSpecial = false;
  } var start = entry.Start;
  if (start) {
    var startDate = new Date(start);
    newSession.Date = startDate;
    newSession.StartTime = startDate;
  } var end = entry.End;
  if (end) {
    var endDate = new Date(end);
    newSession.EndTime = endDate;
  } var speakerIDs = entry.Speakers;
  if (speakerIDs) {
    var speakers = speakerIDs.split(",");
    var sp;
    for (sp = 0; sp < speakers.length; sp++) {
      if (speakers[sp].charAt(0) == " ") {
        speakers[sp] = speakers[sp].substr(1);
      }
    }
    newSession.Speakers = speakers;
  } var regionIDs = entry.Regions;
  if (regionIDs) {
    var regions = regionIDs.split(",");
    var rg;
    for (rg = 0; rg < regions.length; rg++) {
      if (regions[rg].charAt(0) == " ") {
        regions[rg] = regions[rg].substr(1);
      }
    }
    newSession.Regions = regions;
  }
  sessions.push(newSession);
}
```

重构后，代码可以从 100 多行减少到大约 20 行。数据结构很容易理解，转换逻辑也很简单。

```
let sessions = this.programsData.map(program => {
  return {
    ID: program.RowKey,
    Title: program.Title,
    Track: program.Track,
    Type: program.Type,
    Description: program.Description,
    Language: program.Language,
    CalendarLink: program.CalendarLink,
    EventX: program.EventX,
    EventXText: program.EventXText && "Register in EventX",
    Special: program.Special,
    SpecialText: program.SpecialText,
    SpecialIcon: program.SpecialIcon ? "images/em-icons.svg#" +  program.SpecialIcon : "images/em-icons.svg#" + "exclamation-point",
    ShowSpecial: program.Special && program.SpecialText,
    Date: new Date(program.Start),
    StartTime: new Date(program.Start),
    EndTime: new Date(program.End),
    Speakers: program.Speakers ? program.Speakers.split(",").map(speaker => speaker.trim()): [],
    Regions: program.Regions ? program.Regions.split(",").map(region => region.trim()) : []
  }
});
```

## 不使用数组方法—查找

这个例子类似，但更短。这只是为了找到选定的会话。

```
getSession(ID) { for (var i = 0; i < this.sessionList.length; i++) {
    if (this.sessionList[i] && this.sessionList[i].ID && this.sessionList[i].ID == ID) {
      return this.sessionList[i];
    }
  } //No session associated with that ID
  return null;
}
```

这可以用内置数组方法在一行中完成。

```
getSession(ID) {
  return this.sessionList.find(session => session.ID === ID);
}
```

## 手动追加 CSS 类

这只是显示或隐藏 HTML 文档的一部分。强制性地，这可以通过手动操作类列表来完成，但是当查看 HTML 元素时这并不明显。

```
<div class="em-c-alert **em-u-is-hidden**" role="alert" :id="s.ID">
  ...
</div>openLink(ID){
  var element = document.getElementById(ID);
  element.classList.remove("**em-u-is-hidden**");
}closeLink(ID) {
  var element = document.getElementById(ID);
  element.classList.add("**em-u-is-hidden**");
  element.tabindex = -0;
}
```

由于应用程序已经用 Vue 实现，我们可以简单地绑定变量来切换 CSS 类。

```
<div class="em-c-alert" v-bind:class="{ '**em-u-is-hidden**': **!s.isLinkDisplayed** }" role="alert" :id="s.ID">
  ...
</div>openLink(session){
  session.isLinkDisplayed = true;
}closeLink(session) {
  session.isLinkDisplayed = false;
}
```

# 单一组件中的多种职责

一个组件中有 6 个子页面(主页、项目、项目详情、团队、演讲者详情、常见问题)。该组件使用布尔标志自行管理路由，而不是将其分解成自己的组件并通过 [Vue 路由器](https://router.vuejs.org/)管理路由。概念概述如下:

*   用户试图访问子页面(通过单击链接或手动输入 URL)
*   每个链接都映射到自己的 URL，但都链接到同一个组件！！！用不同的道具。
*   在组件内部，根据道具设置布尔标志，即`profileView, profilesView, sessionsView, sessionView, aboutView, FAQView` 。它稍后使用这些标志来显示/隐藏组件中的子页。

这导致一个巨大的组件包含 4350 行代码！！！这是非常繁琐的。

# 未使用的依赖项

最初，我对大量的包依赖关系感到惊讶。我使用 [depcheck](https://www.npmjs.com/package/depcheck) 来分析依赖项，这样我就可以决定删除。不幸的是，有相当多的错误警报——依赖项正在被使用，但被报告为未使用。为了克服这些错误警报，我需要删除并重新启动应用程序，以验证它是否工作正常。最后，我可以去掉一半的依赖。

# 死代码

应移除注释代码。