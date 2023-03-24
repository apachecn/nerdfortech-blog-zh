# 如何刷新 Kivy Recycleview

> 原文：<https://medium.com/nerd-for-tech/how-to-refresh-kivy-recycleview-72244883d075?source=collection_archive---------3----------------------->

您需要以下方法:。刷新来自数据()。更新 recycleview 中的数据，然后刷新 recycleview。您需要将 id 添加到。kv 文件。

中更新和刷新 recycleview 数据。py 文件:

```
.py class HomeScreen(Screen):def data_from_dataset(self):
    # ---- format your data for recycleview here ---- #def refresh_recycleview(self): # ----- Update the recycleview data in the .kv file: ---- #
     self.ids.recycleview_id.data = self.data_from_dataset()

     #  ---- refresh the recycleview: ---- #
    self.ids.recycleview_id.refresh_from_data()
```

使用 on_pre_enter: recycleview 在每次用户进入主屏幕时更新:*

```
<HomeScreen>:
    on_pre_enter: self.refresh_recycleview() RecycleView:
        id: recycleview_id
        data: root.data_from_dataset()
        viewclass: "RecycleButton"
        RecycleBoxLayout:
            default_size: None, dp(60)
            default_size_hint: 1, None
            size_hint_y: None
            height: self.minimum_height
            orientation: 'vertical'
```

*请注意，每次打开主屏幕都刷新可能会导致优化问题，您可能希望仅在数据编辑后更新。我的程序很小，我不需要担心刷新会降低应用程序的性能。