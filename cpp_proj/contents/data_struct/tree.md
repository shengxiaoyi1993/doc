# tree 

## 定义

树指的是N个有父子关系的节点的有限集合，满足以下关系：

　　1、当N=0时，该节点集合为空，这棵树被称为空树。

　　2、在任意的非空树中，有且仅有一个根节点。

　　3、当N>1时，除根节点以外的其余节点可分为M个互为相交的有限集合T1,T2...Tm，其中的每个集合本身又是一棵树并成为根的子树



- 概念

```
节点深度：对任意节点x，x节点的深度表示为根节点到x节点的路径长度。所以根节点深度为0，第二层节点深度为1，以此类推
节点高度：对任意节点x，叶子节点到x节点的路径长度就是节点x的高度
树的深度：一棵树中节点的最大深度就是树的深度，也称为高度
父节点：若一个节点含有子节点，则这个节点称为其子节点的父节点
子节点：一个节点含有的子树的根节点称为该节点的子节点
节点的层次：从根节点开始，根节点为第一层，根的子节点为第二层，以此类推
兄弟节点：拥有共同父节点的节点互称为兄弟节点
度：节点的子树数目就是节点的度
叶子节点：度为零的节点就是叶子节点
祖先：对任意节点x，从根节点到节点x的所有节点都是x的祖先（节点x也是自己的祖先）
后代：对任意节点x，从节点x到叶子节点的所有节点都是x的后代（节点x也是自己的后代）
森林：m颗互不相交的树构成的集合就是森林
```


## 种类

https://blog.csdn.net/u014532217/article/details/79118023

```
无序树
有序树
二叉树
 
https://blog.csdn.net/yixianfeng41/article/details/52802855
满二叉树
完全二叉树
平衡二叉树（AVL） :可以为空树；任意节点的左右两树都是平衡二叉树且高度之差不大于1
二叉查找树（二叉搜索树、BST、二叉排序树） ：任意节点大于左子节点，小于右子节点
霍夫曼树  ：给定n个权值作为n个叶子结点，构造一棵二叉树，若带权路径长度达到最小，称这样的二叉树为最优二叉树，也称为霍夫曼树(Huffman Tree)。
红黑树 ：https://www.jianshu.com/p/e136ec79235c#:~:text=%E7%BA%A2%E9%BB%91%E6%A0%91%E5%AE%9A%E4%B9%89%E5%92%8C%E6%80%A7%E8%B4%A8.%20%E7%BA%A2%E9%BB%91%E6%A0%91%E6%98%AF%E4%B8%80%E7%A7%8D%E5%90%AB%E6%9C%89%E7%BA%A2%E9%BB%91%E7%BB%93%E7%82%B9%E5%B9%B6%E8%83%BD%E8%87%AA%E5%B9%B3%E8%A1%A1%E7%9A%84%E4%BA%8C%E5%8F%89%E6%9F%A5%E6%89%BE%E6%A0%91%E3%80%82.%20%E5%AE%83%E5%BF%85%E9%A1%BB%E6%BB%A1%E8%B6%B3%E4%B8%8B%E9%9D%A2%E6%80%A7%E8%B4%A8%EF%BC%9A.%20%E6%80%A7%E8%B4%A81%EF%BC%9A%E6%AF%8F%E4%B8%AA%E8%8A%82%E7%82%B9%E8%A6%81%E4%B9%88%E6%98%AF%E9%BB%91%E8%89%B2%EF%BC%8C%E8%A6%81%E4%B9%88%E6%98%AF%E7%BA%A2%E8%89%B2%E3%80%82.,%E6%80%A7%E8%B4%A82%EF%BC%9A%E6%A0%B9%E8%8A%82%E7%82%B9%E6%98%AF%E9%BB%91%E8%89%B2%E3%80%82.%20%E6%80%A7%E8%B4%A83%EF%BC%9A%E6%AF%8F%E4%B8%AA%E5%8F%B6%E5%AD%90%E8%8A%82%E7%82%B9%EF%BC%88NIL%EF%BC%89%E6%98%AF%E9%BB%91%E8%89%B2%E3%80%82.%20%E6%80%A7%E8%B4%A84%EF%BC%9A%E6%AF%8F%E4%B8%AA%E7%BA%A2%E8%89%B2%E7%BB%93%E7%82%B9%E7%9A%84%E4%B8%A4%E4%B8%AA%E5%AD%90%E7%BB%93%E7%82%B9%E4%B8%80%E5%AE%9A%E9%83%BD%E6%98%AF%E9%BB%91%E8%89%B2%E3%80%82.%20%E6%80%A7%E8%B4%A85%EF%BC%9A%E4%BB%BB%E6%84%8F%E4%B8%80%E7%BB%93%E7%82%B9%E5%88%B0%E6%AF%8F%E4%B8%AA%E5%8F%B6%E5%AD%90%E7%BB%93%E7%82%B9%E7%9A%84%E8%B7%AF%E5%BE%84%E9%83%BD%E5%8C%85%E5%90%AB%E6%95%B0%E9%87%8F%E7%9B%B8%E5%90%8C%E7%9A%84%E9%BB%91%E7%BB%93%E7%82%B9%E3%80%82.%20%E4%BB%8E%E6%80%A7%E8%B4%A85%E5%8F%88%E5%8F%AF%E4%BB%A5%E6%8E%A8%E5%87%BA%EF%BC%9A.
红黑树使用红黑二色进行“着色”，目的是利用颜色值作为二叉树的平衡对称性的检查，只要插入的节点“着色”满足红黑二色的规定，最短路径与最长路径不会相差的太远，红黑树的节点分布就能大体上达至均衡。
B-tree（B-树或者B树）
 类似于二叉查找树：左孩<父节点<右孩；一个节点可以包含多个关键字；一个节点包含的孩数为(关键字数+1)
 应用：搜索；对于为何选择B树，而不用二叉搜索树，因为B树从硬盘加载的次数更少。（从硬盘加载的速度与树的高度有关；B树节点存储的数据量与硬盘每个扇区存储的容量有关）

B+树
 同B树，但是子节点只保存关键字索引和子节点指针，其他数据保存导叶子节点
 所有关键都会出现在叶子指针中
B*树
是B+树的变体，在B+树的非根和非叶子结点再增加指向兄弟的指针；
B*树分配新结点的概率比B+树要低，空间使用率更高
```
