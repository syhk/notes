# DFS (深度优先遍历）


# BFS（广度优先搜索遍历）

##### 判断链表是否有环：
快慢指针思路：有环时，快慢指针相遇时 （fast==slow) ，然后 fast指针 回到头结点，慢指针在原地，接下来两个指针都走一步，这次相遇一定是在入环的结点处相遇。无环时，fast 指针一定会先走到 nullptr
```cpp
ListNode* detectCycle(ListNode* head) {
    if (head == nullptr || head->next == nullptr) return nullptr;
    int fastcount = 0, slowcount = 0;
    ListNode* slow = head;
    ListNode* fast = head;
    while (true) {
        // 两指针相遇
        if (slow == fast) {
            break;
        }
        fast = fast->next->next;
        slow = slow->next;
    }
    fast = head; // 快指针回到开头

    while (slow != fast) {
        slow = slow.next;
        fast = fast.next;
    }
    return fast;
}
};
```
单向链表一个有环一个无环，它们两个根本不可能相交。

### Tree 相关
非递归遍历
```java
import java.util.Stack;
public class Tree1 {
    // 非递归版本遍历
    // 先序
    /*
* 根节点入栈，终止条件：栈不为空
* 1.每次从栈中弹出一个节点 car
* 2.打印（处理） cat
* 3.先右再左依次压入栈
* 4.重复以上操作
*/
    public static void posOrderUnRecur1(Node head) {
        if (head == null) {
            return;
        }
        Stack<Node> nodestack = new Stack<Node>();
        nodestack.add(head);
        while (nodestack.isEmpty()) {
            head = nodestack.pop();
            System.out.println(head.value + '\t');
            if (head.right != null)
                nodestack.add(head.right);
            if (head.left != null)
                nodestack.add(head.left);
        }
    }
    // 后序
    /*
* 两个栈：
* 1.弹栈 car
* 2.cur放入收集栈
* 3.先左再右依次压入栈
* 4.循环执行以上操作
*/
    public static void posOrderUnRecur2(Node head) {
        if (head == null)
            return;
        Stack<Node> s1 = new Stack<>();
        Stack<Node> s2 = new Stack<>();
        s1.push(head);
        while (!s1.isEmpty()) {
            head = s1.pop();
            s2.push(head);
            if (head.left != null)
                s1.push(head.left);
            if (head.right != null)
                s1.push(head.right);
        }
        // 打印收集栈里面的所有元素
        while (!s2.isEmpty()) {
            System.out.println(s2.pop().value + '\t');
        }
    }
    // 中序
    /*
* 思路：从根节点开始，一路向左走并入栈，直到走到 null
* 打印，把当前弹出节点的右孩子入栈，重复以上过程
*/
    public static void posOrderUnRecur3(Node head) {
        if (head == null)
            return;
        Stack<Node> stack = new Stack<>();
        while (!stack.isEmpty() || head != null) {
            if (head != null) {
                stack.push(head);
                head = head.left; // 往左走
            } else {
                head = stack.pop();// 出栈打印
                System.out.println(head.value + '\t');
                head = head.right;// 再走到弹出节点的右孩子上
            }
        }
    }
}
// 特化 int
class Node {
    int value;
    Node left;
    Node right;
};

// 求树的最大宽度
public static int maxwidth(Node head) {
    if (head == null)
    return 0;
Queue<Node> queue = new LinkedList<>();
queue.add(head);
HashMap<Node, Integer> levelMap = new HashMap<>();// 记录节点与对应的层次
levelMap.put(head, 1);
int curLevel = 1; // 当前所在层
int curLevelNodes = 0;// 当前层的结点总数
int max = Integer.MIN_VALUE;// Integer 的最小值 -2147483648
while (!queue.isEmpty()) {
    Node cur = queue.poll();
    int curNodeLevel = levelMap.get(cur);
    if (curNodeLevel == curLevel) {
        curLevelNodes++;
    } else {
        max = Math.max(max, curLevelNodes);
        curLevel++;
        curLevelNodes = 1;
    }
    if (cur.left != null) {
        levelMap.put(cur.left, curNodeLevel + 1);
        queue.add(cur.left);
    }
    if (cur.right != null) {
        levelMap.put(cur.right, curNodeLevel + 1);
        queue.add(cur.right);
    }
}
return max;
}

```

```java
// 如何判断一颗是否搜索二叉树 BST
// 中序遍历的结果一定是升序的
// 就是把中序遍历的过程中打印值的代码替换成判断是否符合条件的过程
public static int preValue = Integer.MIN_VALUE;

public static boolean isBST(Node head) {
    if (head == null)
    return true;
boolean isLeftBst = isBST(head.left);
if (!isLeftBst)
    return false;
if (head.value <= preValue)
    return false;
else
    preValue = head.value;
return isBST(head.right);
}

// 判断一颗二叉树是否完全二叉树
// 层序遍历：判断条件：1.任一节点有右无左 ->false
// 2. 在满足1的条件下，如果遇到了第一个左右孩子不全，那么后面遇到的全部节点必须全部是叶子节点
public static boolean isCBT(Node head) {
    if (head == null)
    return true;
LinkedList<Node> queue = new LinkedList<>();
// 是否遇到过左右两个孩子不全的节点
boolean leaf = false;
Node Lchild = null;
Node Rchild = null;
queue.add(head);
while (!queue.isEmpty()) {
    head = queue.poll();
    Lchild = head.left;
    Rchild = head.right;
    // 进行判断
    if ((leaf && (Rchild != null && Lchild != null))// 已经发第一个孩子不双全的节点之后当前节点不是叶子节点
        || (Lchild == null && Rchild != null)) {
        return false;
    }
}
// 左孩子入队
if (Lchild != null)
    queue.add(Lchild);
// 右孩子入队
if (Rchild != null)
    queue.add(Rchild);
// 判断双孩子不全
if (Lchild == null || Rchild == null)
    leaf = true;
return true;
}
// 判断一颗是否是满二叉树
public static boolean isFull(Node head) {
    if (head == null)
    return true;
Info data = process3(head);
return data.nodes == ((1 << data.height) - 1);
}

public static Info process3(Node x) {
    if (x == null) {
    return new Info(0, 0);
}
Info leftData = process3(x.left);
Info rightData = process3(x.right);
int height;
int nodes;
height = Math.max(leftData.height, rightData.height) + 1;
nodes = leftData.nodes + rightData.nodes + 1;
return new Info(height, nodes);
}

public static class Info {
    public int height;
    public int nodes;

    public Info(int height, int nodes) {
        this.height = height;
        this.nodes = nodes;
    }
}

// 判断一颗树是否是平衡二叉树
public static boolean isBalanced(Node head) {
    return process(head).isBalanced;
}

public static class ReturnType {
    public boolean isBalanced;
    public int height;

    public ReturnType(boolean b, int i) {
        isBalanced = b;
        height = i;
    }
}

public static ReturnType process(Node x) {
    if (x == null)
    return new ReturnType(true, 0);
ReturnType leftData = process(x.left);
ReturnType rightData = process(x.right);
int height = Math.max(leftData.height, rightData.height);
boolean isBalanced = leftData.isBalanced && rightData.isBalanced
    && Math.abs(leftData.height - rightData.height) < 2;
return new ReturnType(isBalanced, height);
}

public static class ReturnData {
    public boolean isBST;
    public int min;
    public int max;

    public ReturnData(boolean isBST, int min, int max) {
        this.isBST = isBST;
        this.min = min;
        this.max = max;
    }
}

public static ReturnData process2(Node x) {
    if (x == null)
    return null;
ReturnData leftData = process2(x.left);
ReturnData rightData = process2(x.right);

int min = x.value;
int max = x.value;
if (leftData != null) {
    min = Math.min(min, leftData.min);
    max = Math.max(max, leftData.max);
}

if (rightData != null) {
    min = Math.min(min, rightData.min);
    max = Math.max(max, rightData.max);
}

boolean isBST = true;
if (leftData != null && (!leftData.isBST || leftData.max >= x.value)) {
    isBST = false;
}
if (rightData != null && (!rightData.isBST || x.value >= rightData.min)) {
    isBST = false;
}
return new ReturnData(isBST, min, max);
}
```

```java
// o1 和 o2 一定属于 head 为头的树
// 返回 o1 和 o2 的最低公共祖先
public static Node lca(Node head, Node o1, Node o2) {// Lower Common Ancestor lCA
    /**
* 情况： o1 和 o2 的 LCA or o2 是 o1 的 LCA or o1 与 o2 不互为 LCA 需要汇聚才能找到
*/
    if (head == null || head == o1 || head == o2)
        return head;
    Node left = lca(head.left, o1, o2); // 遇到 o1 or o2 or null 会直接返回
    Node right = lca(head.right, o1, o2);// 遇到 o1 or o2 or null 会直接返回
    if (left != null && right != null) {
        return head;
    }
    return left != null ? left : right;
}

// 在二叉树中找到一个节点的后继节点(中序遍历中一个节点的后一个节点)
public static Node getSuccessorNode(Node node) {
    if (node == null)
    return node;
if (node.right != null) {
    return getLeftMost(node.right);
} else { // 无右子树
    Node parent = node.parent;
    while (parent != null && parent.left != node) // 当前节点是其父亲节点右孩子
        {
            node = parent;
            parent = node.parent;
        }
    return parent;
}
}

public static Node getLeftMost(Node node) {
    if (node == null)
    return node;
while (node.left != null) {
    node = node.left;
}
return node;
}

// 二叉树的序列化和反序列化
// 内存里面的一棵树如何变成字符串形式，如何从字符串变成内存里的树
// 以 head 为头的树，序列化成字符串返回
public static String serialByPre(Node head) {
    if (head == null)
    return "#_"; // #代表当前节点为 null
String res = head.value + "_";
res += serialByPre(head.left);
res += serialByPre(head.right);
return res;
}

// 反序列化
public static Node reconByPreString(String preStr) {
    String[] values = preStr.split("_");
Queue<String> queue = new LinkedList<>();
for (var i : values) {
    queue.add(i);
}
return reconPreOrder(queue);
}

public static Node reconPreOrder(Queue<String> queue) {
    String value = queue.poll();
    if (value.equals("#"))
        return null;
    Node head = new Node(Integer.valueOf(value));
    head.left = reconPreOrder(queue);
    head.right = reconPreOrder(queue);
    return head;
}

/**
* 折纸问题：把一段纸条竖着放在桌子上，然后从纸条的下边向上方对折 1 次，压出折痕后展开
* 此时折痕是凹下去的，折痕突起的方向指向纸条的背面
* 如果从纸条的下边向上方连续对折 2 次，奉献出折痕后展开，有三条折痕
* 从下到下依次是下折痕，下折痕和上折痕
* 给定参数 N 输出从上到下打印所有折痕的方向。
* 
* @param N 纸条对折 N 次
*/
public static void printAllFolds(int N) {
    printProcess(1, N, true);
}

public static void printProcess(int i, int N, boolean down) {
    if (i > N)
        return;
    printProcess(i + 1, N, true);
    System.out.println(down ? "凹" : "凸");
    printProcess(i + 1, N, false);

}
```


## Graph 
结构：
```java
package main.test.java.graph;

public class CreateGraph {

    public static Graph createGraph(Integer[][] matrix) {
        Graph graph = new Graph();
        for (int i = 0; i < matrix.length; i++) {
            Integer from = matrix[i][0];
            Integer to = matrix[i][1];
            Integer weight = matrix[i][2];
            if (!graph.nodes.containsKey(from)) {
                graph.nodes.put(from, new Node(from));
            }
            if (!graph.nodes.containsKey(to)) {
                graph.nodes.put(to, new Node(to));
            }
            Node fromNode = graph.nodes.get(from);
            Node toNode = graph.nodes.get(to);
            Edge newEdge = new Edge(weight, fromNode, toNode);
            fromNode.nexts.add(toNode);
            fromNode.out++;
            toNode.in++;
            fromNode.edges.add(newEdge);
            graph.edges.add(newEdge);
        }
        return graph;
    }
}

```
```java
package main.test.java.graph;

// 图结构边的描述
public class Edge {
	public int weight;
	public Node from;
	public Node to;

	public Edge(int weight, Node from, Node to) {
		this.weight = weight;
		this.from = from;
		this.to = to;
	}

}

```
```java

public class Graph {
	public HashMap<Integer, Node> nodes;
	public HashSet<Edge> edges;

	public Graph() {
		nodes = new HashMap<>();
		edges = new HashSet<>();
	}
}

```
```java
public class Node {
	public int value;
	public int in;
	public int out;
	public ArrayList<Node> nexts;// 从自己出发直接关联的点
	public ArrayList<Edge> edges;// 一个自己所拥有的边

	public Node(int value) {
		this.value = value;
		in = 0;
		out = 0;
		nexts = new ArrayList<>();
		edges = new ArrayList<>();
	}
}
```
graph algorithms :
```java
// BFS
/**
* 1.利用队列实现
* 2.从 node 开始依次按照宽度进队列，然后弹出
* 3.每弹出一个点，把该节点所有没有进过队列的邻接点放入队列
* 4.直到队列为空
* 
* @param node
*/
public static void bfs(Node node) {
    if (node == null)
    return;
Queue<Node> queue = new LinkedList<>();
HashSet<Node> set = new HashSet<>();// 已经访问过的结点集合
queue.add(node);
set.add(node);
while (!queue.isEmpty()) {
    Node cur = queue.poll();
    System.out.println(cur.value);
    for (Node next : cur.nexts)// 访问直接相关联的结点
        {
            if (!set.contains(next)) {
                set.add(next);
                queue.add(next);
            }
        }
}
}

/**
* 1.利用栈实现
* 2.从 node 开始把节点按照深度放入栈，然后弹出
* 3.每弹出一个点，把该节点下一个没有进过栈的邻接点入栈
* 4.直到栈为空
* 
* @param node
*/
public static void dfs(Node node) {
    if (node == null)
    return;
Stack<Node> stack = new Stack<>();
HashSet<Node> set = new HashSet<>();
stack.add(node);
set.add(node);
System.out.println(node.value);
while (!stack.isEmpty()) {
    Node cur = stack.pop();
    for (Node next : cur.nexts) {
        if (!set.contains(next)) {
            stack.push(cur);
            stack.push(next);
            set.add(next);
            System.out.println(next.value);
            break;
        }
    }
}

}

// 拓扑排序算法:找到第一个入度为 0 的结点加入到队列，然后擦除这个节点的影响，继承寻找其他结点
// 图的要求： directed graph (有向图) and no loop
public static List<Node> sortedTopology(Graph graph) {
    // key 某个 node value 剩余的入度
    HashMap<Node, Integer> inMap = new HashMap<>();
// 入度为 0 的点，才能进入队列
Queue<Node> queue = new LinkedList<>();
for (Node node : graph.nodes.values()) {
    inMap.put(node, node.in);
    if (node.in == 0) {
        queue.add(node);
    }
}
// 拓扑排序结果
List<Node> result = new ArrayList<>();
while (!queue.isEmpty()) {
    Node cur = queue.poll();
    result.add(cur);
    // 擦除当前节点的影响
    for (Node next : cur.nexts) {
        inMap.put(next, inMap.get(next) - 1);
        if (inMap.get(next) == 0) {
            queue.add(next);
        }
    }
}
return result;
}

// 算法要求：无向图 求最小生成树
// Kruskal : 使用并查集结构 （从边的角度）
// 下面只满足功能要求（模拟并查集）并不是并查集结构
public static class MySets {
    public HashMap<Node, List<Node>> setMap;

    public MySets(List<Node> nodes) {
        for (Node cur : nodes) {
            List<Node> set = new ArrayList<>();
            set.add(cur);
            setMap.put(cur, set);
        }
    }

    public boolean isSameSet(Node from, Node to) {
        List<Node> fromSet = setMap.get(from);
        List<Node> toSet = setMap.get(to);
        return fromSet == toSet;
    }

    public void union(Node from, Node to) {// 把两个集合并为一个集合
        List<Node> fromSet = setMap.get(from);
        List<Node> toSet = setMap.get(to);

        for (Node tNode : toSet) {
            fromSet.add(tNode);
            setMap.put(tNode, fromSet);
        }

    }

}

// 把边按照从小到大的次序考虑，看加上这条边是否形成环
// 没有形成环则添加到结果中，并合并结果集中的结点，形成环就不加入到结果集中
public static Set<Edge> KruskalMST(Graph graph) {
    UnionFind union = new UnionFind();
union.makeSets(graph.nodes.values());
PriorityQueue priorityQueue = new PriorityQueue<>(new EdgeComparator());
for (Edge edge : graph.edges) {
    priorityQueue.add(edge);
}
Set<Edge> result = new HashSet<>();
while (!priorityQueue.isEmpty()) {
    Edge edge = priorityQueue.poll(); // 每次取出边权值最小的边
    if (!union.isSameSet(edge.from, edge.to)) {
        result.add(edge);
        union.union(edge.from, edge.to);
    }
}
return result;
}

// Prim 算法 要求无向图（从点的角度）
// 剪枝策略加贪心
// 在每次解锁的边中选择
权值最小的
public static Set<Edge> primMST(Graph graph) {
    // 解锁的边进入小根堆
    PriorityQueue<Edge> priorityQueue = new PriorityQueue<>(new EdgeComparator());
HashSet<Node> set = new HashSet<>();
Set<Edge> result = new HashSet<>(); // 挑选好的边放入 result 里
for (Node node : graph.nodes.values()) // 随便挑一个点
    {
        // node 开始
        if (!set.contains(node)) {
            set.add(node);
            for (Edge edge : node.edges) // 一个点，解锁所有相连的边
                {
                    priorityQueue.add(edge);
                }
            while (!priorityQueue.isEmpty()) {
                Edge edge = priorityQueue.poll(); // 弹出已解锁边中最小的边
                Node tNode = edge.to; // 可能的一个新点
                if (!set.contains(tNode)) {
                    set.add(tNode);
                    result.add(edge);
                    for (Edge nextEdge : tNode.edges) {
                        priorityQueue.add(nextEdge);// 相关联的边进行解锁
                    }
                }
            }
        }
    }
}

// Dijkstra 算法 (单元最短路径算法)
// 图：可以有负权值的边,但不能有累加和为负数的环
// 规定出发点：到图中所有点的最短路径

public static HashMap<Node, Integer> dijkstra1(Node head) {
    // 从 head 出发到所有点的最小距离
    // key 从 head 出发到达 key
    // value 从 head 出发到达 key 的最小距离
    // 如果在表中，没有 T 的记录，含义是从 head 出发到 T 这个点的距离为正无穷
    HashMap<Node, Integer> distanceMap = new HashMap<>();
distanceMap.put(head, 0);
// 记录求过距离的节点，存在 selectedNode 中
HashSet<Node> selectedNodes = new HashSet<>();
Node minNode = getMinDistanceAndselectedNode(distanceMap, selectedNodes);

while (minNode != null) {
    int distance = distanceMap.get(minNode);
    for (Edge edge : minNode.edges) {
        Node toNode = edge.to;
        if (!distanceMap.containsKey(toNode)) {
            distanceMap.put(toNode, distance + edge.weight);
        }
        distanceMap.put(edge.to, Math.min(distanceMap.get(toNode), distance + edge.weight));
    }
    selectedNodes.add(minNode);
    minNode = getMinDistanceAndselectedNode(distanceMap, selectedNodes);

}
return distanceMap;
}

public static Node getMinDistanceAndselectedNode(
    HashMap<Node, Integer> distanceMap,
    HashSet<Node> touchedNodes) {
    Node minNode = null;
    int minDistance = Integer.MAX_VALUE;
    for (java.util.Map.Entry<Node, Integer> entry : distanceMap.entrySet()) {
        Node node = entry.getKey();
        int distance = entry.getValue();
        if (!touchedNodes.contains(node) && distance < minDistance) {
            minNode = node;
            minDistance = distance;
        }
    }
    return minNode;
}
```

#### 前缀和
应用场景：原始数组不会被修改的情况下，频繁查询某个区间的累加和。<br />**一维前缀和**：题目链接：<br />[https://leetcode.cn/problems/range-sum-query-immutable/](https://leetcode.cn/problems/range-sum-query-immutable/)
```cpp
{ sum[i] = sum[i-1] + arr[i]  , i > 0
{ sum[0] = arr[0]             , i = 0
```
**一维差分：**<br />**适用场景：频繁对原始数组的某个区间的元素进行增减。**
```cpp
[L,R] + V <=> d[L] + V , d[R+1] - V
//把标记后的差分数组进行一次前缀后操作
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668910706273-e419461e-34f8-4b6d-aabf-b1246a7d2f77.png#averageHue=%23f8f7f5&clientId=u79c1c544-5943-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u357d3a28&margin=%5Bobject%20Object%5D&name=image.png&originHeight=720&originWidth=1280&originalType=url&ratio=1&rotation=0&showTitle=false&size=604875&status=done&style=none&taskId=u7e39cf7a-acea-43dc-aad2-39160ee093e&title=)<br />**diff[i] += 3 意味着给 nums[i..] 所有的元素都加了 3，然后 diff[j+1] -= 3 又意味着对于 nums[j+1..] 所有元素再减 3，那综合起来，是不是就是对 nums[i..j] 中的所有元素都加 3 了**

```java
package main.test.java.test;

// 差分数组工具类
public class Difference {
	// 差分数组
	private int[] diff;

	// 输入一个初始数组，区间操作将在这个数组上进行
	public Difference(int[] nums) {
		assert nums.length > 0;
		diff = new int[nums.length];
		// 构造差分数组
		diff[0] = nums[0];
		for (int i = 1; i < nums.length; i++) {
			diff[i] = nums[i] - nums[i - 1];
		}
	}

	// 给闭区间 [i,j] 增加 value
	public void increment(int i, int j, int value) {
		diff[i] += value;
		if (j + 1 < diff.length) {
			diff[j + 1] -= value;
		}
	}

	// 返回结果数组
	public int[] result() {
		int[] res = new int[diff.length];
		// 根据差分数组构造结果数组
		res[0] = diff[0];
		for (int i = 1; i < diff.length; i++) {
			res[i] = res[i - 1] + diff[i];
		}
		return res;
	}

	public static void main(String args[]) {
		int[] arr = new int[] { 1, 4, 5, 7, 9, 80 };
		Difference difference = new Difference(arr);
		difference.increment(2, 4, 100);
		difference.increment(1, 3, -1000);
		int[] res = difference.result();
		for (var i : res) {
			System.out.println(i);
		}

	}
}

```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668912882621-53ed9950-ba23-4812-a310-887aa380e357.png#averageHue=%23e3f2fd&clientId=u79c1c544-5943-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=810&id=ua6deced3&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1013&originWidth=507&originalType=binary&ratio=1&rotation=0&showTitle=false&size=69215&status=done&style=none&taskId=ua580a248-f53b-49ab-861b-9f48fad0cb4&title=&width=405.6)<br />题目：<br />[https://leetcode.cn/problems/car-pooling/submissions/](https://leetcode.cn/problems/car-pooling/submissions/)
```cpp
// 利用差分数组进行求解
// 题目说 n 的最大范围为 1000
   int ans[1001]={0};
   for(auto &v : trips)
    {
     ans[v[1]] += v[0];
     ans[v[2]] -= v[0];
    }
    int now = 0;
   for( int i = 0 ; i <= 1000 ; i ++ )
    {
     now+=ans[i];
     if(now > capacity) 
        {
           return false;
        } 
    }
  return true;
  }

```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668916111313-c9caeae9-1c63-4746-ba39-524eff40b2c3.png#averageHue=%23e6f4fe&clientId=u79c1c544-5943-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=527&id=u3ffe265a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=659&originWidth=764&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46782&status=done&style=none&taskId=u1dfacf95-50b0-41ba-8239-9d2979874aa&title=&width=611.2)<br />矩阵：<br />[https://leetcode.cn/problems/rotate-image/](https://leetcode.cn/problems/rotate-image/)
```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        // 先沿对角线反转二维矩阵
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                // swap(matrix[i][j], matrix[j][i]);
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
        // 然后反转二维矩阵的每一行
        for (int[] row : matrix) {
            reverse(row);
        }
    }

    // 反转一维数组
    void reverse(int[] arr) {
        int i = 0, j = arr.length - 1;
        while (j > i) {
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
            i++;
            j--;
        }
    }
}
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668923989608-c3d5ca6e-16cc-4ce0-b516-61eef2dc816d.png#averageHue=%23e6f4fe&clientId=u79c1c544-5943-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=587&id=u2060c066&margin=%5Bobject%20Object%5D&name=image.png&originHeight=734&originWidth=634&originalType=binary&ratio=1&rotation=0&showTitle=false&size=57808&status=done&style=none&taskId=u22686f9d-4add-455a-ab51-73cc14028ae&title=&width=507.2)
```cpp
void rotate(vector<vector<int>>& matrix) {
    for (int i=0; i<matrix.size(); i++)
        for (int j=0; j<i; j++) swap(matrix[i][j], matrix[j][i]); // 先进行对角线镜像处理
    for (auto& row: matrix) reverse(row.begin(), row.end()); // 再依次反转每行
}
```
```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        // 先沿对角线反转二维矩阵
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                // swap(matrix[i][j], matrix[j][i]);
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
        // 然后反转二维矩阵的每一行
        for (int[] row : matrix) {
            reverse(row);
        }
    }

    // 反转一维数组
    void reverse(int[] arr) {
        int i = 0, j = arr.length - 1;
        while (j > i) {
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
            i++;
            j--;
        }
    }
}
```
>  快速排序就是个二叉树的前序遍历，归并排序就是二叉树的后序遍历 

前序遍历代码执行是自顶向下，而后序是自底向上的<br />前序位置的代码只能从函数参数中获取父节点传递过来的数据，而后序位置不仅可以获取参数数据，还可以获取子树通过函数返回值传递回来的数据。<br />二叉树问题思考过程：

- 是否可以通过遍历一遍二叉树得到答案？
- 是否可以定义一个递归函数，通过子问题的答案推导出原问题的解
- 思考清楚二叉树的每一个节点需要做什么，需要在什么时候做？（前中后层）

### 前缀树<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668931429926-e9eb85a7-e555-462c-98f5-120bc5526637.png#averageHue=%23e6f4fe&clientId=u79c1c544-5943-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=392&id=uaab064e8&margin=%5Bobject%20Object%5D&name=image.png&originHeight=490&originWidth=435&originalType=binary&ratio=1&rotation=0&showTitle=false&size=35846&status=done&style=none&taskId=ua1876eb5-ad35-4f06-829f-1516e1b66a2&title=&width=348)
```java
package main.test.java.Trie;

// 前缀树
public class TrTree {

    public static class TrieNode {
        public int pass;// 经过当前节点就加 1
        public int end; // 标记一个单词一结尾
        public TrieNode[] nexts;

        public TrieNode() {
            pass = 0;
            end = 0;
            nexts = new TrieNode[26]; // 一共只会 26 条路
        }
    }

    public static class Trie {

        private TrieNode root;

        public Trie() {
            root = new TrieNode();
        }

        public void insert(String word) {
            if (word == null) {
                return;
            }
            char[] chs = word.toCharArray();
            TrieNode node = root;
            node.pass++;
            int index = 0;
            for (int i = 0; i < chs.length; i++)// 从左往左遍历字符
                {
                    // index 的结果会在范围 0-25 之间
                    index = chs[i] - 'a'; // 由字符，对应成走向哪条路
                    if (node.nexts[index] == null) { // 没有节点
                        node.nexts[index] = new TrieNode();
                    }
                    node = node.nexts[index]; // 如果有直接复用
                    node.pass++;
                }
            node.end++;
        }

        // 传递进来的单词加入过几次
        public int search(String word) {
            if (word == null)
                return 0;
            char[] chs = word.toCharArray();
            TrieNode node = root;
            int index = 0;
            for (int i = 0; i < chs.length; i++) {
                index = chs[i] - 'a';
                if (node.nexts[index] == null) {
                    return 0;
                }
                node = node.nexts[index];
            }
            return node.end;
        }

        // 查询有多少单词是 str 为前缀的
        public int prefix(String pre) {
            if (pre == null)
                return 0;
            char[] chs = pre.toCharArray();
            TrieNode node = root;
            int index = 0;
            for (int i = 0; i < chs.length; i++) {
                index = chs[i] - 'a';
                if (node.nexts[index] == null) {
                    return 0;
                }
                node = node.nexts[index];
            }
            return node.pass;
        }

        // 删除：沿路的 pass -- ，最后一个 end -- 也就是单词结尾的 end --
        public void delete(String word) {
            if (search(word) != 0)// 确定树中加入过 word 才删除
            {
                char[] chs = word.toCharArray();
                TrieNode node = root;
                int index = 0;
                for (int i = 0; i < chs.length; i++) {
                    index = chs[i] - 'a';
                    if (--node.nexts[index].pass == 0) {
                        // c++ 需要析构
                        node.nexts[index] = null;
                        return;
                    }
                    node = node.nexts[index];
                }
                node.end--;
            }
        }
    }
}

```

### 贪心
**局部最优 -?-> 整体最优**
> **题目：一些项目要占用一个会议室宣讲，会议室不能同时容纳两个项目宣讲。给每一个项目开始的时间和结束的时间安排宣讲的日程，要求会议室进行的宣讲的场次最多并返。**

```java
public static class Program {
    public int start;
    public int end;
}

public static class ProgramComparator implements Comparator<Program> {

    @Override
    public int compare(Program o1, Program o2) {
        // TODO Auto-generated method stub
        return o1.end - o2.end;
    }
}

// 安排会议 , 选择结束时间最早的
public static int bestArrange(Program[] programs, int timePoint) {
    Arrays.sort(programs, new ProgramComparator());
    int result = 0;
    // 从左向右依次遍历所有的会议
    for (int i = 0; i < programs.length; i++) {
        if (timePoint <= programs[i].start) {
            result++;
            timePoint = programs[i].end;
        }
    }
    return result;
}
```

```java
/**
* 题目：一块金条切成两半，需要花费和长度数值一样的铜板。
* eg: 长为 20 的金条，不管切成哪两半都需要花费 20 个铜板
* 问：一群人想切分整块金条，怎么分最省铜板？
* 
* @param arr arr 元素和是金条的长度。其中各个元素代表金条需要分成的 n 个部分
* @return 花的铜板数
*/
// 思路：就是在构建哈夫曼树的过程
public static int lessMoney(int[] arr) {
    PriorityQueue<Integer> queue = new PriorityQueue<>();
    for (int i = 0; i < arr.length; i++) {
        queue.add(arr[i]);
    }
    int sum = 0;
    int cur = 0;
    while (queue.size() > 1) {
        cur = queue.poll() + queue.poll();
        sum += cur;
        queue.add(cur);
    }
    return sum;
}
```


























