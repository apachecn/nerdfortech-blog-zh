# 在 Python 系列中构建森林:AVL 树 vs 红黑树

> 原文：<https://medium.com/nerd-for-tech/build-the-forest-in-python-series-avl-tree-vs-red-black-tree-2c5d8c316443?source=collection_archive---------19----------------------->

成为一名优秀的软件工程师不仅需要知道有什么工具(例如，数据结构和算法)可用，还需要知道如何选择正确的工具。此外，一个好的软件工程师还需要知道如何衡量我们选择的工具是否如我们所期望的那样工作。本文不打算像前几篇文章那样构建一个新的树数据结构。相反，我们将构建一个工具，一个名为 *Metrics、*的 Python 库，它可以监控软件行为和性能，然后我们可以使用该工具来证明我们的 AVL 树和红黑树应该工作。虽然我们使用 AVL 树和红黑树作为例子，但是我们测量这些树的思想可以应用于任何软件和真实世界的情况。

AVL 树和红黑树经常被相互比较，因为它们都为其核心运算提供了时间复杂度:O(lg n)，其中 n 是节点数(在文章的其余部分，如果没有提到，n 表示节点数)。许多教科书和文章已经从理论上证明了这两种树的性能和复杂性。然而，计算机软件(或一般的计算机科学)是一门实用的学科。有许多方法可以实现相同的思想、算法和数据结构。此外，我们还需要修改我们的实现以适应我们的实际约束，例如内存限制，这意味着实现并不严格遵循最初的定义。带着这种想法，本文试图以一种更实际的方式来看待这两棵树——通过使用我们将要构建的*度量*库来测量和比较它们。

**我们衡量什么？**

AVL 树和红黑树保持平衡的关键是旋转，所以我们将测量插入和删除发生时的旋转次数。此外，我们还想监测树高的变化，这样我们就知道树一直是平衡的。

**怎么做呢？**

如上所述，我们将使用*度量*库来跟踪我们的树。如果我们想测量软件的行为，我们可以在软件中注入一些代码，这段代码会一直跟踪软件的行为。在这里，我们将构建的*度量*库是完成这项工作的一段代码。

# 理论上 AVL 树与红黑树的比较

在我们构建*度量*库之前，让我们基于它们的定义来分析 AVL 树和红-黑 Tee。我们将使用结论作为事实的来源，通过*度量*库生成的结果来验证我们的实现。

**搜索**

为了找到一个节点，我们需要在最坏的情况下将树从它的根走到它的叶级，这需要 O(h)的时间来找到节点，其中 h 是树的高度(如果没有提到，h 表示高度)。

红黑树确保没有路径比其他路径长两倍，而 AVL 树保证对于 AVL 树中的每个节点，其左子树和右子树的高度最多相差一。所以 AVL 树比红黑树更平衡。换句话说，AVL 树通常比红黑树具有更快的查找速度。如果我们的用例需要很多查找，AVL 树可能是比红黑树更好的解决方案。

**插入**

要插入一个新节点，我们首先找到要删除的节点的父节点，这需要 O(h)时间，然后执行旋转以保持树的平衡，这需要恒定的时间。然而，红黑树比 AVL 树需要更少的旋转，因此我们可以说红黑树比 AVL 树具有更快的插入速度。

**删除**

要删除一个节点，我们首先花 O(h)时间找到要删除的节点，然后执行旋转以保持树的平衡。像插入一样，AVL 树比红黑树需要更多的旋转来平衡它。比插入更糟糕的是，红黑树具有恒定的最大旋转次数，但是 AVL 树可以进行 O(h)次旋转。在最坏的情况下，AVL 树可能需要执行从违反 AVL 树属性的节点到根的旋转。所以红黑树的删除速度比 AVL 树快。如果场景是大量的插入和删除，红黑树比 AVL 树有更好的性能。

**空间使用量**

这两棵树的空间使用率都是 O(n)。它们都比一般的二叉查找树节点存储更多的信息。红黑树在其节点中存储颜色信息，而 AVL 树在节点中存储每个节点的高度。然而，这两棵树的实际空间开销有点不同。由于颜色只能是红色或黑色，所以它可以存储为布尔值，甚至只是一个位(即 0 或 1)。

相反，一个节点的高度是一个整数，这意味着我们必须将它存储为一个整数。这个事实在 Python 实现中没有太大的区别，因为布尔类型和整数类型具有相同的大小。我们可以使用内置的 [getsizeof](https://docs.python.org/3/library/sys.html#sys.getsizeof) 函数来检查它们的字节大小。

```
>>> import sys
>>> color: bool = True  # True indicates Red; False means Black
>>> height: int = 10  # The tree height is an integer
>>> sys.getsizeof(color)
28
>>> sys.getsizeof(height)
28
```

但是，在其他语言中，大小可能会有很大不同。例如，在 C++中，我们可以将颜色存储为大小为 8 位的 *char* 值，将高度存储为 32 位整数( *int32_t* )(有关 C++类型的更多详细信息，请参见[基本类型](https://en.cppreference.com/w/cpp/language/types))。因此，在 C++实现中，AVL 树节点消耗的空间是红黑树节点的四倍。

因为空间使用在 Python 实现中没有太大的区别，所以我们不会在实验中追踪空间使用。

# 项目设置

除了在[构建森林系列](https://shunsvineyard.info/build-the-forest-series/)中使用的相同风格和假设(Python 3.9 或更新版本)之外，我们还假设 AVL 树和红黑树的用例是单线程，整个树可以放在 RAM 中。本文向我们的项目添加了两个模块: *metrics.py、*Metrics library 和 *test_metrics.py* 作为它的单元测试。添加这两个文件后，我们的项目布局如下:

```
forest-python
├── forest
│   ├── __init__.py
│   ├── binary_trees
│   │   ├── __init__.py
│   │   ├── avl_tree.py
│   │   ├── binary_search_tree.py
│   │   ├── double_threaded_binary_tree.py
│   │   ├── red_black_tree.py
│   │   ├── single_threaded_binary_trees.py
│   │   └── traversal.py
│   ├── metrics.py
│   └── tree_exceptions.py
└── tests
    ├── __init__.py
    ├── conftest.py
    ├── test_avl_tree.py
    ├── test_binary_search_tree.py
    ├── test_double_threaded_binary_tree.py
    ├── test_metrics.py
    ├── test_red_black_tree.py
    ├── test_single_threaded_binary_trees.py
    └── test_traversal.py
```

# 韵律学

[Metrics](https://metrics.dropwizard.io/4.1.2/) 项目启发了我们将要构建的 Metrics library，它有两个主要组件: *MetricRegistry* 和支持的度量类型，包括*计数器*和*直方图*。

# 公制计量法

*MetricRegistry* 是使用*指标库*的起点，它是用于跟踪应用程序的所有指标的集合。因此，举例来说，如果我们需要跟踪一个操作，比如转数，我们可以定义一个*计数器*类型的度量来监控转数，并将*计数器*度量注册到 *MetricRegistry* 中，用一个可读的名称 *avlt.rotations* 。使用这种方法，我们可以很容易地管理指标，客户端代码可以通过指标的名称通过 *MetricRegistry* 检索任何指标。

实现很简单。我们使用 Python 字典来保存注册的指标。键是指标的名称，值是指标实例。为了类型安全，我们还为支持的类型定义了 *MetricType* 。

```
MetricType = Union[Counter, Histogram]
"""Alias for the supported metric types."""

class MetricRegistry:
    """A registry for metric instances."""

    def __init__(self) -> None:
        self._registry: Dict[str, MetricType] = dict()

    def register(self, name: str, metric: MetricType) -> None:
        """Given a metric, register it under the given name.

        Parameters
        ----------
        name: `str`
            The name of the metric

        metric: `MetricType`
            The type of the metric
        """
        self._registry[name] = metric

    def get_metric(self, name: str) -> MetricType:
        """Return the metric by the given name.

        Parameters
        ----------
        name: `str`
            The name of the metric

        Returns
        -------
        `MetricType`
            The metric instance by the given name.
        """
        return self._registry[name]
```

# 计数器

*计数器*是一个简单的度量单位，用于计数，即递增和递减计数器。任何需要计数的东西都可以使用*计数器*——比如转数。

```
class Counter:
    """An incrementing and decrementing counter metric."""

    def __init__(self) -> None:
        self._count = 0

    def increase(self, n: int = 1) -> None:
        """Increment the counter.

        Parameters
        ----------
        n: `int`
            The count to be increased.
        """
        self._count += n

    def decrease(self, n: int = 1) -> None:
        """Decrement the counter.

        Parameters
        ----------
        n: `int`
            The count to be decreased.
        """
        self._count -= n

    @property
    def count(self) -> int:
        """Return the current count."""
        return self._count
```

# 柱状图

根据定义，直方图是数字数据分布的图形表示。它是对连续变量(定量变量)概率分布的估计。*度量*库中的*直方图*测量数据集的分布，包括最小值、平均值、最大值、标准偏差和百分位数。

因为我们假设我们测量的软件可以放在 RAM 中，所以我们可以使用 Python List 来保存所有的值，并使用 [Numpy](https://numpy.org/doc/stable/) 来计算数据集的分布。

```
import numpy as np

class Histogram:
    """A metric which calculates the distribution of a value."""

    def __init__(self) -> None:
        self._values: List[int] = list()

    def update(self, value: int) -> None:
        """Add a recorded value.

        Parameters
        ----------
        value: `int`
            value to be updated
        """
        self._values.append(value)

    def report(self) -> Dict:
        """Return the histogram report."""
        array = np.array(self._values)
        return {
            "min": array.min(),
            "max": array.max(),
            "medium": np.median(array),
            "mean": array.mean(),
            "stdDev": array.std(),
            "percentile": {
                "75": np.percentile(array, 75),
                "95": np.percentile(array, 95),
                "99": np.percentile(array, 99),
            },
        }
```

(完整的实现可从 [metrics.py](https://github.com/shunsvineyard/forest-python/blob/main/forest/metrics.py) 获得)

# 试验

和往常一样，我们应该尽可能多地对代码进行单元测试。检查 [test_metrics.py](https://github.com/shunsvineyard/forest-python/blob/main/tests/test_metrics.py) 进行完整的单元测试。

# 将指标放入树中

# 红黑树

在我们实现了*度量*库之后，我们可以将度量放在红黑树类中。因为我们想测量旋转的次数，所以我们用一个*计数器*来测量。为了计算树高的变化，我们可以使用一个*直方图*。此外，我们需要一个 *MetricRegistry* 来注册*计数器*和*直方图*。 *MetricRegistry* 支持管理整个程序中的指标，所以更高层应该将 *MetricRegistry* 实例传递给 *RBTree* 类。这样，我们修改 *RBTree* 的 *__init__* 函数如下。

```
class RBTree:
    def __init__(self, registry: Optional[metrics.MetricRegistry] = None) -> None:
        self._NIL: Leaf = Leaf()
        self.root: Union[Node, Leaf] = self._NIL
        self._metrics_enabled = True if registry else False
        if self._metrics_enabled and registry:
            self._rotate_counter = metrics.Counter()
            self._height_histogram = metrics.Histogram()
            registry.register(name="rbt.rotate", metric=self._rotate_counter)
            registry.register(name="rbt.height", metric=self._height_histogram)
```

(完整实现见 [red_black_tree.py](https://github.com/shunsvineyard/forest-python/blob/main/forest/binary_trees/red_black_tree.py)

需要注意的一件事是，尽管通过像 *Metrics* library 这样的方法来洞察软件行为是必要的，但这也是有代价的。它增加了代码的复杂性，并可能降低其性能，因为它需要做更多的事情。因此，我们将*注册表的*参数设置为可选。当我们不度量代码时，我们可以禁用度量以避免潜在的副作用。

**旋转计数器**

顾名思义，当旋转发生时，我们需要增加计数器的值。因此，我们调用 *increase()* 函数在旋转结束时的函数如下。

```
def _left_rotate(self, node_x: Node) -> None:
    node_y = node_x.right  # Set node y
    if isinstance(node_y, Leaf):  # Node y cannot be a Leaf
        raise RuntimeError("Invalid left rotate")
    # Turn node y's subtree into node x's subtree
    node_x.right = node_y.left
    if isinstance(node_y.left, Node):
        node_y.left.parent = node_x
    node_y.parent = node_x.parent
    # If node's parent is a Leaf, node y becomes the new root.
    if isinstance(node_x.parent, Leaf):
        self.root = node_y
    # Otherwise, update node x's parent.
    elif node_x == node_x.parent.left:
        node_x.parent.left = node_y
    else:
        node_x.parent.right = node_y
    node_y.left = node_x
    node_x.parent = node_y

    if self._metrics_enabled:
        self._rotate_counter.increase()

def _right_rotate(self, node_x: Node) -> None:
    node_y = node_x.left  # Set node y
    if isinstance(node_y, Leaf):  # Node y cannot be a Leaf
        raise RuntimeError("Invalid right rotate")
    # Turn node y's subtree into node x's subtree
    node_x.left = node_y.right
    if isinstance(node_y.right, Node):
        node_y.right.parent = node_x
    node_y.parent = node_x.parent
    # If node's parent is a Leaf, node y becomes the new root.
    if isinstance(node_x.parent, Leaf):
        self.root = node_y
    # Otherwise, update node x's parent.
    elif node_x == node_x.parent.right:
        node_x.parent.right = node_y
    else:
        node_x.parent.left = node_y
    node_y.right = node_x
    node_x.parent = node_y

    if self._metrics_enabled:
        self._rotate_counter.increase()
```

(完整实现见 [red_black_tree.py](https://github.com/shunsvineyard/forest-python/blob/main/forest/binary_trees/red_black_tree.py)

**高度直方图**

在我们执行插入和删除后，高度可能会发生变化，我们想要跟踪的是树高的趋势。因此，我们可以像下面这样在插入和删除结束时更新*直方图*。

```
def insert(self, key: Any, data: Any) -> None:
    # Color the new node as red.
    new_node = Node(key=key, data=data, color=Color.RED)
    parent: Union[Node, Leaf] = self._NIL
    current: Union[Node, Leaf] = self.root
    while isinstance(current, Node):  # Look for the insert location
        parent = current
        if new_node.key < current.key:
            current = current.left
        elif new_node.key > current.key:
            current = current.right
        else:
            raise tree_exceptions.DuplicateKeyError(key=new_node.key)
    new_node.parent = parent
    # If the parent is a Leaf, set the new node to be the root.
    if isinstance(parent, Leaf):
        new_node.color = Color.BLACK
        self.root = new_node
    else:
        if new_node.key < parent.key:
            parent.left = new_node
        else:
            parent.right = new_node
        # After the insertion, fix the broken red-black-tree-properties.
        self._insert_fixup(new_node)

    if self._metrics_enabled:
        self._height_histogram.update(value=self.get_height(self.root))

def delete(self, key: Any) -> None:
    if (deleting_node := self.search(key=key)) and (
        isinstance(deleting_node, Node)
    ):
        original_color = deleting_node.color
        # Case 1: no children or Case 2a: only one right child
        if isinstance(deleting_node.left, Leaf):
            replacing_node = deleting_node.right
            self._transplant(
                deleting_node=deleting_node, replacing_node=replacing_node
            )
            # Fixup
            if original_color == Color.BLACK:
                if isinstance(replacing_node, Node):
                    self._delete_fixup(fixing_node=replacing_node)
        # Case 2b: only one left child
        elif isinstance(deleting_node.right, Leaf):
            replacing_node = deleting_node.left
            self._transplant(
                deleting_node=deleting_node, replacing_node=replacing_node
            )
            # Fixup
            if original_color == Color.BLACK:
                if isinstance(replacing_node, Node):
                    self._delete_fixup(fixing_node=replacing_node)
        # Case 3: two children
        else:
            replacing_node = self.get_leftmost(deleting_node.right)
            original_color = replacing_node.color
            replacing_replacement = replacing_node.right
            # The replacing node is not the direct child of the deleting node
            if replacing_node.parent == deleting_node:
                if isinstance(replacing_replacement, Node):
                    replacing_replacement.parent = replacing_node
            else:
                self._transplant(replacing_node, replacing_node.right)
                replacing_node.right = deleting_node.right
                replacing_node.right.parent = replacing_node
            self._transplant(deleting_node, replacing_node)
            replacing_node.left = deleting_node.left
            replacing_node.left.parent = replacing_node
            replacing_node.color = deleting_node.color
            # Fixup
            if original_color == Color.BLACK:
                if isinstance(replacing_replacement, Node):
                    self._delete_fixup(fixing_node=replacing_replacement)

        if self._metrics_enabled:
            self._height_histogram.update(value=self.get_height(self.root))
```

(完整实现见 [red_black_tree.py](https://github.com/shunsvineyard/forest-python/blob/main/forest/binary_trees/red_black_tree.py)

有了*直方图*,经过一些插入和删除，我们可以得到像最大值、平均值和中等树高这样的报告。通过将这些数字与标准偏差和百分位数相结合，我们可以清楚地描绘出身高的变化。例如，如果我们有 99%的百分位数接近中值，标准偏差相对较低，我们可以说树高相当一致。换句话说，这棵树相当平衡。

# AVL 树

与红黑树类似，我们使用一个*计数器*来跟踪旋转，使用一个*直方图*来监控树的高度。

```
class AVLTree:
    def __init__(self, registry: Optional[metrics.MetricRegistry] = None) -> None:
        self.root: Optional[Node] = None
        self._metrics_enabled = True if registry else False
        if self._metrics_enabled and registry:
            self._rotate_counter = metrics.Counter()
            self._height_histogram = metrics.Histogram()
            registry.register(name="avlt.rotate", metric=self._rotate_counter)
            registry.register(name="avlt.height", metric=self._height_histogram)
```

(完整实现见 [avl_tree.py](https://github.com/shunsvineyard/forest-python/blob/main/forest/binary_trees/avl_tree.py) )

我们还在旋转函数的末尾增加了*计数器*，并在插入和删除后更新了*直方图*。

```
def insert(self, key: Any, data: Any) -> None:
    new_node = Node(key=key, data=data)
    parent: Optional[Node] = None
    current: Optional[Node] = self.root
    while current:
        parent = current
        if new_node.key < current.key:
            current = current.left
        elif new_node.key > current.key:
            current = current.right
        else:
            raise tree_exceptions.DuplicateKeyError(key=new_node.key)
    new_node.parent = parent
    # If the tree is empty, set the new node to be the root.
    if parent is None:
       self.root = new_node
    else:
        if new_node.key < parent.key:
            parent.left = new_node
        else:
            parent.right = new_node
        if not (parent.left and parent.right):
            self._insert_fixup(new_node)

    if self._metrics_enabled:
        self._height_histogram.update(value=self.get_height(self.root))

def delete(self, key: Any) -> None:
    if self.root and (deleting_node := self.search(key=key)):
        # Case: no child
        if (deleting_node.left is None) and (deleting_node.right is None):
            self._delete_no_child(deleting_node=deleting_node)
        # Case: Two children
        elif deleting_node.left and deleting_node.right:
            replacing_node = self.get_leftmost(node=deleting_node.right)
            # Replace the deleting node with the replacing node,
            # but keep the replacing node in place.
            deleting_node.key = replacing_node.key
            deleting_node.data = replacing_node.data
            if replacing_node.right:  # The replacing node cannot have left child.
                self._delete_one_child(deleting_node=replacing_node)
            else:
                self._delete_no_child(deleting_node=replacing_node)
        # Case: one child
        else:
            self._delete_one_child(deleting_node=deleting_node)

        if self._metrics_enabled:
            self._height_histogram.update(value=self.get_height(self.root))

def _left_rotate(self, node_x: Node) -> None:
    node_y = node_x.right  # Set node y
    if node_y:
        # Turn node y's subtree into node x's subtree
        node_x.right = node_y.left
        if node_y.left:
            node_y.left.parent = node_x
        node_y.parent = node_x.parent
        # If node's parent is a Leaf, node y becomes the new root.
        if node_x.parent is None:
            self.root = node_y
        # Otherwise, update node x's parent.
        elif node_x == node_x.parent.left:
            node_x.parent.left = node_y
        else:
            node_x.parent.right = node_y
        node_y.left = node_x
        node_x.parent = node_y
        node_x.height = 1 + max(
            self.get_height(node_x.left), self.get_height(node_x.right)
        )
        node_y.height = 1 + max(
            self.get_height(node_y.left), self.get_height(node_y.right)
        )

        if self._metrics_enabled:
            self._rotate_counter.increase()

def _right_rotate(self, node_x: Node) -> None:
    node_y = node_x.left  # Set node y
    if node_y:
        # Turn node y's subtree into node x's subtree
        node_x.left = node_y.right
        if node_y.right:
            node_y.right.parent = node_x
        node_y.parent = node_x.parent
        # If node's parent is a Leaf, node y becomes the new root.
        if node_x.parent is None:
            self.root = node_y
        # Otherwise, update node x's parent.
        elif node_x == node_x.parent.right:
            node_x.parent.right = node_y
        else:
            node_x.parent.left = node_y
        node_y.right = node_x
        node_x.parent = node_y
        node_x.height = 1 + max(
            self.get_height(node_x.left), self.get_height(node_x.right)
        )
        node_y.height = 1 + max(
            self.get_height(node_y.left), self.get_height(node_y.right)
        )

        if self._metrics_enabled:
            self._rotate_counter.increase()
```

(完整实现见 [avl_tree.py](https://github.com/shunsvineyard/forest-python/blob/main/forest/binary_trees/avl_tree.py)

# 二叉查找树

尽管本文主要关注 AVL 树和红黑树，但是同样的方法也很容易应用到二叉查找树。由于二叉查找树没有旋转操作，我们只需要一个*直方图*来测量树高。在下一节中，我们可以将二叉查找树添加到比较中，以更好地理解自平衡树如何比二叉查找树表现得更好。

(参见 [binary_search_tree.py](https://github.com/shunsvineyard/forest-python/blob/main/forest/binary_trees/binary_search_tree.py) 通过向二叉查找树添加指标来实现)

# 用指标比较红黑树和 AVL 树

# 实验 1

在第一个实验中，我们使用 Python 内置的随机采样函数( [random.sample](https://docs.python.org/3/library/random.html#random.sample) )来生成一个列表( *insert_data* )，其中包含从 1 到 2000 范围内选择的 1000 个唯一的随机整数。并使用与*删除 _ 数据*相同的 *random.sample* 函数随机化*插入 _ 数据*。所以从树中删除节点的顺序是随机的。

```
import random

insert_data = random.sample(range(1, 2000), 1000)
delete_data = random.sample(insert_data, 1000)
```

然后，我们为度量定义一个 *MetricRegistry* ，并在定义树时传递它。在这里，我们还定义了一个二叉查找树，以便我们可以看到二叉查找树和自平衡树之间的不同性能。

```
from forest import metrics
from forest.binary_trees import avl_tree
from forest.binary_trees import binary_search_tree
from forest.binary_trees import red_black_tree

registry = metrics.MetricRegistry()
bstree = binary_search_tree.BinarySearchTree(registry=registry)
avltree = avl_tree.AVLTree(registry=registry)
rbtree = red_black_tree.RBTree(registry=registry)
```

一旦树和度量准备好了，我们首先将 *insert_data* 插入到树中，然后按照 *delete_data* 的顺序从树中删除节点。

```
for key in insert_data:
    bstree.insert(key=key, data=str(key))
    avltree.insert(key=key, data=str(key))
    rbtree.insert(key=key, data=str(key))

for key in delete_data:
    bstree.delete(key=key)
    avltree.delete(key=key)
    rbtree.delete(key=key)
```

最后，我们可以通过名称检索度量，并打印出旋转次数和高度的直方图。

```
print("Binary Search Tree:")
bst_report = registry.get_metric(name="bst.height").report()
print(f"  Height:   {bst_report}")

print("AVL Tree:")
avlt_rotation_count = registry.get_metric(name="avlt.rotate").count
print(f"  Rotation: {avlt_rotation_count}")
avlt_report = registry.get_metric(name="avlt.height").report()
print(f"  Height:   {avlt_report}")

print("Red-Black Tree")
rbt_rotation_count = registry.get_metric(name="rbt.rotate").count
print(f"  Rotation: {rbt_rotation_count}")
rbt_repot = registry.get_metric(name="rbt.height").report()
print(f"  Height:   {rbt_repot}")
```

(完整代码可在 [avlt_vs_rbt.py](https://github.com/shunsvineyard/forest-python/blob/main/examples/avlt_vs_rbt.py) 获得)

由于输入数据是随机生成的，所以每次的结果可能会略有不同，但结果应该是非常接近的。输出可能如下所示:

```
Binary Search Tree:
  Height:   {'min': 0, 'max': 21, 'medium': 17.0, 'mean': 16.52976488244122, 'stdDev': 3.621953722665703, 'percentile': {'75': 20.0, '95': 20.0, '99': 21.0}}
AVL Tree:
  Rotation: 1053
  Height:   {'min': -1, 'max': 11, 'medium': 10.0, 'mean': 9.1765, 'stdDev': 1.7370514528936674, 'percentile': {'75': 10.0, '95': 11.0, '99': 11.0}}
Red-Black Tree
  Rotation: 647
  Height:   {'min': 0, 'max': 12, 'medium': 11.0, 'mean': 9.9605, 'stdDev': 1.78295814589126, 'percentile': {'75': 11.0, '95': 12.0, '99': 12.0}}
```

从输出中，我们可以看到 AVL 树和红黑树都非常平衡——AVL 树的最大高度是 11，红黑树是 12，中间值和平均值也非常接近它们的最大高度。而且，它们的百分位数是相对于中值的，标准差相对较低。因此，我们可以说这两棵树有一致的高度。

相反，二叉查找树的树高更差——最大 21，中等 17。我们知道，基于我们在[二叉查找树分析](https://shunsvineyard.info/2021/03/13/build-the-forest-in-python-series-binary-search-tree/#19-analysis)中提到的定理:在 n 个不同的键上随机构建二叉查找树的期望高度是 O(lg n)。即使我们随机生成二叉查找树，它的树高仍然比 AVL 树和红黑树差。此外，二叉查找树的标准差比 AVL 树和红黑树更显著。在这里，我们表明，即使我们随机生成二叉查找树，AVL 树和红黑树也比二叉查找树更平衡。

关于旋转次数，结果显示 AVL 树比红黑树需要更多的旋转来保持平衡，符合我们的预期。

# 实验二

我们在第二次实验中会比第一次实验更有侵略性。我们随机生成插入和删除数据，并以随机顺序执行插入和删除。同样，我们使用 [random.sample](https://docs.python.org/3/library/random.html#random.sample) 函数生成一个列表( *insert_data* )，其中包含从 1 到 3000 范围内选择的 2000 个唯一的随机整数。但是这一次，我们用 1000 个从 1 到 3000 的唯一随机整数生成 *delete_data* 。在这种情况下，插入和删除数据是不同的。这意味着当我们试图删除一个节点时，该节点可能不存在，但这不是问题。只要我们使用相同的 *insert_data* 和 *delete_data* 以相同的顺序测试我们的树，实验就是有效的。下面的代码显示了我们如何生成测试数据。

```
import random

sample_data = random.sample(range(1, 3000), 2000)
insert_data = [("insert", item) for item in sample_data]

sample_data = random.sample(range(1, 3000), 1000)
delete_data = [("delete", item) for item in sample_data]

test_data = random.sample(
    (insert_data + delete_data), len(insert_data) + len(delete_data)
)
```

我们通过 *random.sample* 函数将 *insert_data* 和 *delete_data* 合并成一个 *test_data* ，所以顺序是随机的。此外， *insert_data* 和 *delete_data* 的每一项都是一对，包含操作类型(插入或删除)和要插入或删除的数据。因此，我们知道在使用操作类型检查 test_data 时，需要执行哪些操作，插入还是删除。这就是我们如何确保对所有三棵树使用相同的数据集和相同的顺序。下面的代码演示了这个实验。

```
from forest import metrics
from forest.binary_trees import avl_tree
from forest.binary_trees import binary_search_tree
from forest.binary_trees import red_black_tree

registry = metrics.MetricRegistry()
bstree = binary_search_tree.BinarySearchTree(registry=registry)
avltree = avl_tree.AVLTree(registry=registry)
rbtree = red_black_tree.RBTree(registry=registry)

for operation, key in test_data:
    if operation == "insert":
        bstree.insert(key=key, data=str(key))
        avltree.insert(key=key, data=str(key))
        rbtree.insert(key=key, data=str(key))
    if operation == "delete":
        bstree.delete(key=key)
        avltree.delete(key=key)
        rbtree.delete(key=key)

print("Binary Search Tree:")
bst_report = registry.get_metric(name="bst.height").report()
print(f"  Height:   {bst_report}")

print("AVL Tree:")
avlt_rotation_count = registry.get_metric(name="avlt.rotate").count
print(f"  Rotation: {avlt_rotation_count}")
avlt_report = registry.get_metric(name="avlt.height").report()
print(f"  Height:   {avlt_report}")

print("Red-Black Tree")
rbt_rotation_count = registry.get_metric(name="rbt.rotate").count
print(f"  Rotation: {rbt_rotation_count}")
rbt_repot = registry.get_metric(name="rbt.height").report()
print(f"  Height:   {rbt_repot}")
```

(完整代码可从 [avlt_vs_rbt_2.py](https://github.com/shunsvineyard/forest-python/blob/main/examples/avlt_vs_rbt_2.py) 获得)

测试数据也是随机生成的，因此每次的结果可能会略有不同。示例输出可能如下所示:

```
Binary Search Tree:
  Height:   {'min': 0, 'max': 23, 'medium': 22.0, 'mean': 20.394120153387302, 'stdDev': 3.249885374276778, 'percentile': {'75': 22.0, '95': 23.0, '99': 23.0}}
AVL Tree:
  Rotation: 1455
  Height:   {'min': 0, 'max': 12, 'medium': 11.0, 'mean': 10.175117170856412, 'stdDev': 1.6120322559944114, 'percentile': {'75': 11.0, '95': 12.0, '99': 12.0}}
Red-Black Tree
  Rotation: 1136
  Height:   {'min': 0, 'max': 13, 'medium': 11.0, 'mean': 10.826161056668086, 'stdDev': 1.8772598891928807, 'percentile': {'75': 12.0, '95': 13.0, '99': 13.0}}
```

我们得到了与实验 1 类似的结果——红黑树比 AVL 树需要更少的旋转，AVL 树比红黑树稍微平衡，AVL 树和红黑树都比二叉查找树平衡得多。

# 摘要

AVL 树和红黑树都广泛应用于计算机软件中。他们每个人都有长处和短处。对于软件开发来说，选择最适合我们用例的软件是至关重要的。此外，知道如何衡量我们的选择和实现对于软件的成功也是至关重要的。本文介绍了一种使用 AVL 树和红黑树作为例子来度量软件行为的方法。虽然本文中示例的范围很小，但是这里使用的概念可以应用于任何软件。希望这篇文章能提供有价值的知识，让人们从中受益。

*原载于 2021 年 7 月 2 日*[*https://shunsvineyard . info*](https://shunsvineyard.info/2021/07/02/build-the-forest-in-python-series-avl-tree-vs-red-black-tree/)*。*