#### 二叉树的性质：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1667956097830-cfdb7375-a4dc-480d-adbe-d7fbb7c52a95.png#averageHue=%23faf9f8&clientId=ue9550380-307f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=122&id=u246e788c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=153&originWidth=811&originalType=binary&ratio=1&rotation=0&showTitle=false&size=53530&status=done&style=none&taskId=uc991b67c-cc7c-4841-9987-e1b3e278bf3&title=&width=648.8)
真二叉树：所有节点的度都要么为0，要么为2
满二叉树：最后一层节点的度都为0，其他节点的度都为2

- 在同样高度的二叉树中，满二叉树的叶子节点数量最多，总节点数量最多
- 满二叉树一定是真二叉树，真二叉树不一定是满二叉树

完全二叉树：对节点从上至下、左到右开始编号，其所有编号都能与相同高度的满二叉树中的编号对应

- 叶子节点只会出现在最后2层，最后1层的叶子结点都靠左对齐
- 完全二叉树从根结点至倒数第2层是一棵满二叉树
- 满二叉树一定是完全二叉树，完全二叉树不一定是满二叉树
- 度为1的节点只有左子树
- 度为1的节点要么是1个，要么是0个
- 同样节点数量的二叉树，完全二叉树的高度最小
- 假设完全二叉树的高度为 h (h>=1) 

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1667956466031-1b7fe055-a257-4d59-90b7-98a807bde8cf.png#averageHue=%23fbf9f8&clientId=ue9550380-307f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=176&id=u9f1e3de5&margin=%5Bobject%20Object%5D&name=image.png&originHeight=220&originWidth=581&originalType=binary&ratio=1&rotation=0&showTitle=false&size=43678&status=done&style=none&taskId=u64bfce71-981f-4700-8304-7c59af9698c&title=&width=464.8)

- 一棵有 n 个节点的完全二叉树（n>0)，从上到下，从




# AVL 平衡二叉搜索树
四种调节情况：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1666832056883-68967967-84b0-4329-80b1-7489fd088bc8.png#averageHue=%23f1eeee&clientId=uc98d5200-b737-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=919&id=ua74842ee&margin=%5Bobject%20Object%5D&name=image.png&originHeight=919&originWidth=1759&originalType=binary&ratio=1&rotation=0&showTitle=false&size=281215&status=done&style=none&taskId=ud04dcb1d-292c-40bb-9cfb-0684562b073&title=&width=1759)


![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1666832477626-79280db8-52d1-4de7-8994-1531688530f2.png#averageHue=%23e6d3c1&clientId=uc98d5200-b737-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=910&id=uefee64bd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=910&originWidth=1700&originalType=binary&ratio=1&rotation=0&showTitle=false&size=358109&status=done&style=none&taskId=u268a052e-92aa-486e-8b76-47331d4fde7&title=&width=1700)

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1666832906687-2ae6abc6-941a-4808-8619-464a9c85b63c.png#averageHue=%23e5d2c0&clientId=uc98d5200-b737-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=887&id=u92978a6e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=887&originWidth=1755&originalType=binary&ratio=1&rotation=0&showTitle=false&size=287753&status=done&style=none&taskId=ub6cd57cd-b232-4c60-b670-3d6b9e5f6b3&title=&width=1755)

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1666832959494-d90e7ebf-93fd-4573-87d7-278f2c903de9.png#averageHue=%23e6d3c1&clientId=uc98d5200-b737-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=876&id=ue7e74d96&margin=%5Bobject%20Object%5D&name=image.png&originHeight=876&originWidth=1704&originalType=binary&ratio=1&rotation=0&showTitle=false&size=292601&status=done&style=none&taskId=ufb69983a-954e-4cb0-9b6a-83f59436df6&title=&width=1704)

# 红黑树
添加一共有 12 种情况：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668056377681-6fec37cf-a8aa-4e39-b028-f2ba60375f53.png#averageHue=%23f3dcd4&clientId=ubd3fcbae-7d94-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=142&id=ue145e0f2&margin=%5Bobject%20Object%5D&name=image.png&originHeight=178&originWidth=604&originalType=binary&ratio=1&rotation=0&showTitle=false&size=43697&status=done&style=none&taskId=u8bb21857-3bc7-4b20-b2b6-abcddc3d309&title=&width=483.2)
有 4  种情况满足 4 阶 B 树的性质：parent 为 BLACK 所以不用做任何处理
有 8 种情况不满足红黑树的属性 4 ：parent 为 RED（Double Red)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668056591905-02709dd4-0dac-4bde-8737-4609d35eac9a.png#averageHue=%23f2e8df&clientId=ubd3fcbae-7d94-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=381&id=ue462b85e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=476&originWidth=870&originalType=binary&ratio=1&rotation=0&showTitle=false&size=166243&status=done&style=none&taskId=u86c93e70-2933-4b0c-895a-4cec8db512e&title=&width=696)


### 哈希表/散列表（ Hash Table)
添加、搜索、删除的流程都是类似的：

- 利用哈希函数生成 key 对应的 index O(1)
- 根据 index 操作定位数组元素 O(1)

典型空间换时间应用
Hash Collision (哈希冲突）：两个不同的 key ， 经过哈希函数计算出相同的结果
解决：

- 开放定址法
   - 按照一定规则向其他地址探测，直到遇到空桶  
- 再哈希法
   - 设计多个哈希函数
- 链地址法
   - 通过链表将同一index 的元素串起来（JDK1.8哈希表容量 >= 64 ,链表的节点数量大于 8时转换为红黑树

**哈希函数大概实现步骤：**

- 先生成 key 的哈希值（必须是整数）
- 再让 key 的哈希值跟数组的大小进行相关运算，生成一个索引值

为了提高效率，可以使用 & 位运算取代 % 运算（前提是数组的长度设计为 2 的幂 2^n）

- 尽量让每个 key  的哈希值是唯一的
- 尽量让 key 的每个信息都参与运算

java 官方对于 Long 和 Double 哈希值的计算方法：
```java
public static  int hashCode(long vlaue)
{
    return (int)(value ^ (value >>> 32)); // >>> 无符号右移 
}

public static  int hashCode(double value)
{
    long bits = doubleToLongBits(value);
	return (int)(bits ^ (bits >>> 32));
}

```
>>> 和 ^ 的作用是： 利用高 32bit 和 低 32bit 混合计算出 32bit 的哈希值
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668235595379-6aa48891-7b01-43ad-aba9-49a41535bfef.png#averageHue=%23e1d4cc&clientId=udf98a9bc-dc57-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=72&id=u1a47d223&margin=%5Bobject%20Object%5D&name=image.png&originHeight=90&originWidth=862&originalType=binary&ratio=1&rotation=0&showTitle=false&size=60783&status=done&style=none&taskId=ua2d37a94-5618-4346-ae6e-220e8c3ab36&title=&width=689.6)

### 哈夫曼树（最优二叉树）
哈夫曼编码：left 为 0 ， right 为 1 ，任意一个编码都不是其它编码的前缀
总结：

- n 个权值构建出来的哈夫曼树拥有 n 个叶子节点
- 每个哈夫曼编码都不是另一个哈曼编码的前缀

 
### Trie 
> 字典树，前缀树，单词查找树

Trie 搜索字符串的效率只跟字符串长度有关，数据量大小不影响
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668312968665-4203971f-19c1-4955-8d3c-d6b04bce1f87.png#averageHue=%23f5e8df&clientId=uc28aa6f7-9d1c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=251&id=ucdc1b108&margin=%5Bobject%20Object%5D&name=image.png&originHeight=314&originWidth=674&originalType=binary&ratio=1&rotation=0&showTitle=false&size=48531&status=done&style=none&taskId=u035b29b2-dbec-4a34-b38d-6004fb34b06&title=&width=539.2)


满二叉树：最后一层结点的度都为 0 ， 其他结点的度都为 2。
完全二叉树：对节点从上至下，左至右开始编号，其所有编号都能与相同高度的满二叉树中的编号对应。
四则表达式：

- 前缀表达式（波兰表达式）
- 中缀表达式
- 后缀表达式（逆波兰表达式）

二叉树的迭代遍历：






















