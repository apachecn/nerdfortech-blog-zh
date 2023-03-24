# 终止流程—第 77 天(Python)

> 原文：<https://medium.com/nerd-for-tech/kill-process-day-77-python-b75bcb6ac65e?source=collection_archive---------4----------------------->

![](img/c36ddff0991c3bc7aa228faff1a3ddcb.png)

塞巴斯蒂安·斯塔姆在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

今天的问题来自 Leetcode 的每日编码挑战二月版。是 Leetcode 上一个中等标签的问题。让我们看看问题陈述。

[T5 582](https://leetcode.com/problems/kill-process/)**。杀死过程**

给定 **n 个**进程，每个进程有一个唯一的 **PID(进程 id)** 和它的 **PPID(父进程 id)** 。

每个进程只有一个父进程，但可能有一个或多个子进程。这就像一个树形结构。只有一个进程的 PPID 为 0，这意味着该进程没有父进程。所有 PID 都将是不同的正整数。

我们使用两个整数列表来表示一个进程列表，其中第一个列表包含每个进程的 PID，第二个列表包含相应的 PPID。

现在给定这两个列表，以及一个表示您想要终止的进程的 PID，返回最终将被终止的进程的 PID 列表。您应该假设当一个进程被终止时，它的所有子进程都会被终止。最终答案不需要排序。

**例 1:**

```
**Input:** 
pid =  [1, 3, 10, 5]
ppid = [3, 0, 5, 3]
kill = 5
**Output:** [5,10]
**Explanation:** 
           3
         /   \
        1     5
             /
            10
Kill 5 will also kill 10.
```

**注:**

1.  给定的 kill id 肯定是给定的 PID 之一。
2.  n >= 1。

开始解决这个问题的一种方法是使用一种数据结构来跟踪所有节点的子节点。我们可以使用字典来存储父子关系。

让我们看看如何存储这种关系。

```
parent_dictionary = collections.defaultdict(list)
for p in range(len(ppid)):
    parent_dictionary[ppid[p]].append(pid[p])
```

接下来，我们需要确定所有必须被杀死的节点。我们从输入节点开始。我们看看谁是这个节点的子节点。然后我们寻找这些孩子的孩子。继续寻找孩子，直到我们到达叶节点。看来我们需要使用 BFS 或 DFS 来解决这个问题。

我们将使用 BFS 来解决这个问题。

我们需要一个队列来跟踪哪个进程需要被终止。我们将从队列中取出第一个节点，在字典中查找它的子节点，并在队列中添加子节点。不断重复这些步骤，直到队列为空。每次我们从队列中删除节点时，都会将它添加到输出列表中。

```
import collections
class ProcessKiller:
    def killProcess(self, pid: List[int], ppid: List[int], kill: int) -> List[int]:
        parent_dictionary = collections.defaultdict(list)
        output = []
        next_process_to_kill = [kill]
        for p in range(len(ppid)):
            parent_dictionary[ppid[p]].append(pid[p])
        while(next_process_to_kill):
            kill_proc = next_process_to_kill.pop(0)
            output.append(kill_proc)
            for process in parent_dictionary[kill_proc]:
                next_process_to_kill.append(process)
        return output
```

**复杂性分析。**

**时间复杂度**

我们正在访问 ppid 和 pid 中的每个节点来创建字典。如果我们必须杀死根节点，杀死过程的时间将为 O(N)。因此时间复杂度是 O(N)。

**空间复杂性。**

因为我们使用字典来存储节点，所以空间复杂度是 O(N)，其中 N 是节点的数量。

我们也可以使用 DFS 来解决这个问题。由于我们已经使用 DFS 解决了相当多的问题，我们可以直接查看代码片段。

```
import collections
class ProcessKiller:
    def killProcess(self, pid: List[int], ppid: List[int], kill: int) -> List[int]:
        parent_dictionary = collections.defaultdict(list)
        output = []
        next_process_to_kill = [kill]
        for p in range(len(ppid)):
            parent_dictionary[ppid[p]].append(pid[p])
        def dfs(node):
            output.append(node)
            for child in parent_dictionary[node]:
                dfs(child)
        dfs(kill)
        return output
```

**复杂性分析。**

**时间复杂度**

我们正在访问 ppid 和 pid 中的每个节点来创建字典。如果我们必须杀死根节点，杀死过程的时间将为 O(N)。因此时间复杂度是 O(N)。

**空间复杂性。**

因为我们使用字典来存储节点，所以空间复杂度是 O(N)，其中 N 是节点的数量。