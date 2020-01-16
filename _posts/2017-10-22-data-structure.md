---
title: 数据结构
---

* 串
* 广义表：LS=(a1, a2..., an)，其中ai可以为原子或子表。GetHead：取表头，为非空广义表的第一个元素；GetTail：取表尾，除去表头以外的元素构成的表，即表尾一定是表。

树
--

### 基本术语

* 树的结点（根结点）：包含一个数据元素及若干指向子树的分支
* 孩子结点（子结点）：结点的子树的根称为该结点的孩子； 子孙结点：以某结点为根的子树中任一结点都称为该结点的子孙
* 双亲结点（父结点）； 祖先结点：从根到该结点的所经分支上的所有结点
* 兄弟结点：父结点相同的结点；堂兄结点：同一层上的结点
* 结点层：根结点的层定义为1；根的孩子为第二层结点，依此类推
* 树的深度：树中最大的结点层
* 结点的度：结点子树的个数；树的度： 树中结点最大的度数
* 叶子结点（终端结点）：度为 0 的结点；分支结点：度不为0的结点
* 有序树：子树有序的树，如：家族树、二叉树；无序树：不考虑子树的顺序
* 森林：互不相交的树集合；森林和树之间的联系是：一棵树去掉根，其子树构成一个森林；一个森林增加一个根结点成为树
* 路径：两个结点之间经过的结点序列；路径长度：路径中边的个数

### 性质

* 结点总数=各个度数结点的数量之和，也=分支总数(边数)+1，而分支总数=各结点的度数之和；即N=1+N1+2N2(+3N3)=N0+N1+N2(+N3)
* 森林转换为二叉树：对于某个结点，第一个孩子为(二叉树的)左孩子，右兄弟为(二叉树的)右孩子；画法上是把兄弟连起来，然后转45度；单个树的根结点没有右兄弟，所以树转换成二叉树的根结点没有右子树，只有森林有；这种转换的结果是唯一的；二叉树转为森林类似

### 实现

* 双亲表示法：{ data, parent }
* 孩子表示法：{ data, degree, childs } // degree是该结点的度，可没有
* 孩子兄弟法（即二叉树表示法）：{ firstchild, data, nextsibling }

二叉树
------

### 性质

* 链表的实现：lchild、data、rchild
* 在二叉树的第i 层上最多有2\^(i-1)个结点(i ≥1)
* 满二叉树：高度为h的二叉树，结点数为(2\^h)-1，也是能有的最多的结点
* 设二叉树叶子结点数为n0，度为2的结点n2，则n0 = n2+1，则n=n1+2n2+1，且满二叉树n1为0，完全二叉树n1为0或1
* 线索二叉树：添加ltag和rtag标识是前驱(1)还是孩子(0)，某个结点的前驱/后继就是它中序遍历的前面/后面那个结点（如果是中序线索二叉树）；树是逻辑结构但线索二叉树是一种物理/储存结构；线索的数量：2n0+n1==n+1，但默认最左边和最右边的节点为null，要具体情况具体分析；用处是先序/中序遍历时不再需要栈，但后序仍需要；在先序(后序)线索二叉树中查找一个结点的先序后继(后序前驱)很简单，但查找先序前驱(后序后继)必须知道双亲结点
* 与树的区别：度为2的有序树至少有三个结点，而二叉树可以为空；即使某结点只有一个子结点，二叉树也是要分左右的

### 完全二叉树

* 只有最后一行的后面一部分可以没有结点
* 具有n个结点的完全二叉树或满二叉树的高度：㏒2(n)向下取整+1，或log(n+1)向上取整
* n/2向下取整前面的结点全部是分支结点，其余的为叶子结点
* 如果有度为1的结点，只能有一个且就是最后一个分支结点，且此时n为偶数；n为奇数则所有分支结点的度都为2

### 数组的实现

* 结点N的父结点是N/2，左孩子是2N，右孩子是2N+1；如果N是偶数，它是父结点的左孩子，否则为右孩子。但这样做根结点索引需要从1开始，当数特别大时会造成浪费
* 若要从0开始，结点N的父结点是是(N+1)/2 -1，左孩子是2N+1，右孩子是2N+2

### 遍历

* 先序遍历：先访问自己，然后是左孩子，然后右孩子；中序遍历、后序遍历略。这三种遍历，叶子节点的先后顺序完全相同
* 树的先根遍历等于其对应二叉树表示的的先序遍历，后根遍历等于中序遍历
* 层次遍历：按数组的顺序访问，若是链表的树用队列辅助
* 非递归中序遍历：当p不为null时，入栈，p=p-\>lchild；else出栈并访问p（此时栈可以为空），然后p=p-\>rchild；循环条件是p不为null或栈非空（栈空时右孩子可能不为null）；思路是只管自己，孩子是否为null不管；如果是先序遍历，入栈前访问p即可
* 非递归后续遍历：还是p不为null时入栈走左边，为null时出栈；但是要（在结点属性中）加一个flag记录是否走完了孩子，出栈时检查；如果为false，改为true后再压栈p，p=p-\>rchild，下一次出来时就为true了；如果为true，访问自己后把p=null即可；或在最外面放个指针指向最近访问的结点
* 由遍历序列构造二叉树：根据一棵树的先序和中序序列或者后序和中序序列可唯一地确定一颗二叉树，第一个(最后一个)一定是根结点，到中序序列里找相同的字母就能分出左子树和右子树，反复继续即可；层次和中序序列也可确定；但如果只知道先序和后序序列，无法确定树，只能确定祖先关系：先序XY，后序YX，则X是Y的祖先
* 一个叶子结点是二叉树中某个子树中序遍历结果的最后一个，则它一定也是该子树先序遍历结果的最后一个
* 如果一颗非空的二叉树先序遍历和后续遍历的序列正好相反，则该二叉树一定只有一个叶子结点，结点数与高度相同；先序和后序相同，则只有根节点
* 非递归层次遍历求树的深度：根入队；当队列不为空时，出队列，左右孩子入队列；有一个last记录每一层的最后一个结点，当front==last时，层数加一，last=rear（因为此时队列里有且仅有下一层的所有结点）

并查集
------

一种简单的集合表示，支持以下操作：

1. Union(S, Root1, Root2)：把集合S中的子集合Root2并入子集合Root1中，要求Root1和Root2不相交。用森林表示就是一颗树的根结点加到另一棵树的根结点下，变成直接子结点，且只能对最高层做这个操作。
2. Find(S, x)：查找集合S中，单元素x所在的子集合。注意返回的是最高层结点，理想上所有子结点都是最高层的下一层就是最优的。
3. Initial(S)：将集合S中每一个元素都初始化为只有一个单元素的子集合。

常用双亲表示法作为并查集的储存结构，可以只用一个一维数组，值表示父节点的索引，初始化时全部赋值为-1，合并时把Root2的值改为Root1的索引，Find时循环查找S[x]直到值为-1，返回索引。但这样没有路径优化。

另一种方式是初始化时S[i]=i，检查时当S[i]==i即表示最高层。这样在查找的时候可以把子结点移动到上面去，即路径优化。具体看：https://zhuanlan.zhihu.com/p/35314141

二叉排序树(BST)
---------------

* 左子树所有结点 \< 根结点 \< 右子树所有结点，是一种递归结构。结点含有值，进行比较时可以只比较当前与给定的来决定，不用管左右是否为null
* 查找：返回结点而不是bool，因此如果当前不为null且不等于当前值才往左右走，否则直接返回当前即可
* 插入：如果为null就创建新结点，否则如果和当前相等就直接返回什么也不做，否则如果小于当前或大于当前就递归调用该函数
* 删除：删叶子结点什么也不用做，删度为1的结点就让子树成为父节点的子树。如果度为2，在右子树上找中序第一个后继填补，然后递归删除那个后继
* 与二分查找类似，但二分查找的判定树唯一，BST不唯一，不同的插入顺序会形成不同的BST。二分查找如果用来插入和删除，需要移动结点，而BST只要改指针即可。中序遍历BST可得有序序列，所以只需要知道先序/后序就可画出整个BST了
* 查找效率主要与高度有关。可能出现一长条的情况，此时比较次数会大于1/2（最坏是O(n)）
* 最佳二叉排序树（高度最小）：先把数组排好序，然后按照左闭右开二分法构造。除了最下一层可以不满外，其他各层都是充满了的。是AVL树，但**不是完全二叉树**

### 平衡二叉树（AVL树）

* 左子树和右子树的高度之差的绝对值不超过1
* 左子树和右子树也是平衡二叉树
* 但其实以上两个条件并没有说要左\<中\<右，因此有人认为AVL不属于BST
* 平衡因子：左子树高度-右子树高度，在递归的插入和删除时更新；需2bit，可以存到指针里
* 保证查询的时间复杂度是O(logn)
* 结点数的递推公式：N1=1, N2=2, Nh=1+N(h-1)+N(h-2)，h为高度，Nh为构建此高度的AVL最少需要的结点数，此时所有非叶子结点的平衡因子都为1；图像上表示，就是每次迭代都给孩子未满的结点加一个子节点

#### 插入调整方法

* 每次只对最小的不平衡子树调整，例如插入后导致多个结点平衡因子绝对值大于1，但只需以最下面那个绝对值为2的为基准
* LL平衡旋转（右单旋转）：在结点A的左孩子(L)的左子树(L)上插入了新结点导致A失衡。需将A的左孩子B向右上旋转替代A成为根结点，A向右下旋转成为B的右子树的根结点，B的原右子树作为A结点的左子树；RR左单旋转：略
* LR平衡旋转（先左后右双旋转）：由于在A的左孩子(L)的右子树(R)上插入新结点导致A失衡，需要进行两次旋转操作。先将A的左孩子B左旋（B的右孩子C提升到B），再把A右旋（A的左孩子C提升到A）；RL先右后左：略
* 调整后的结点可能不是叶子结点：一开始根节点和左孩子两个结点，往左孩子的右孩子插入，平衡后就不是叶子结点了

另一种方法：

* LL/RR形：把中间的放到上面
* LRL/RLR形：把中间那个放到最上面去
* LRR/RLL形：把中间那个放到最上面去，但最后一个结点放到另一颗树上，否则不满足左边严格小于右边

#### 删除

* 最坏需要递归到根

#### 红黑树

* 结构改变小，所以红黑树一般可以做可持久化
* 插入是常数，删除也是常数，而AVL删除不是
* 适合插入多的情形，查找比AVL稍微慢
* https://blog.lilydjwg.me/2019/10/3/red-black-trees.214825.html

哈夫曼树
--------

### 概念

* 路径、路径长度：见上文树的概念
* 树的路径长度：从根到每一结点的路径长度之和
* 权
* 结点的带权路径长度：从该结点到根之间的路径长度与结点上权的乘积
* 树的带权路径长度：所有叶子结点的带权路径长度之和，记作WPL

### 构造过程

1. 根据给定的n个权值构造n棵只有根结点的二叉树，n棵构成一个森林F
2. 在F中选取两颗根结点的权值最小的树作为左右子树构造一个新的二叉树，且权值为左右子树之和。
3. 在F中删除那两棵树，把新树加入F。
4. 重复(2)和(3)，直到F只含一棵树为止。
5. 可以用数组实现，也可以用链表。构造时选中的两颗子树，先遍历到的为左子树。新构造的树放在末尾。

### 性质

* 权值越小，根到那个结点的路径长度越大
* 构造过程中新建了N-1个结点（度都为2），因此总结点数为2N-1
* 每次构造都选两颗子树作为新结点的孩子，因此不存在度为1的结点
* 如果是非二叉树的，树的度为m，仍然只有度为0和m两种结点；若叶子结点个数为n，N=N0+Nm=1+m\*Nm，Nm=(n-1)/(m-1)

### 哈夫曼编码

* 哈夫曼编码是最优前缀编码
* 依次以叶子为出发点，向上回溯至根为止。回溯时走左分支则生成代码0，右分支则为1

### 其它应用

* 两两合并多个有序表时，优先合并长度小的，因为每次都会把合并完的再遍历一遍。这就用到了哈夫曼树的构造思想

图
--

### 基本术语

* G=(V,E)，V(G)为非空顶点集，E(G)为边的集；|V|为顶点的个数，叫阶
* 无向图：E={(1,2),(1,3)}。有向图：E={`<1,2>,<2,1>`}，v,w：v为弧尾，w为弧头，从v指向w
* 简单图：不存在重复边、不存在顶点到自身的边。数据结构仅讨论简单图。反之如果两个顶点间的边数多于一条或者有与自己关联的边，则为多重图
* 完全图：无向图有n(n-1)/2条边，任意两个顶点之间都存在边。有向图有n(n-1)条弧，任意两个顶点之间存在方向相反的两条弧。用矩阵表示就是除对角线以外全部都是1
* 子图：略。不是任意V和E的子集都能构成G的子图，当E的子集中有关联了不存在于V的子集的顶点时，就不是一个图。如果V不变，E少了，叫生成子图
* 连通、连通图、连通分量：v到w有**路径**存在称为连通。任意两个顶点连通则为连通图。连通分量指的是**无向非连通图**中的极大连通子图（有多个）。一个有n个顶点的图的边如果少于n-1，必为非连通图
* 极大连通子图：连通子图略。连通图只有一个极大连通子图，就是它本身。非连通图**有多个**极大连通子图。称为极大是此时把任意一个不在子图中的顶点并入子图都会导致这个子图不再连通（那个点和子图之间没有边）
* 强连通图和强连通分量：在**有向图**中，任意两个不同的顶点，Vi-\>Vj和Vj-\>Vi都存在**路径**（不是弧），则为强连通图。边最少的强连通图为一个环
* 连通图的生成树：一个极小连通子图，含有所有顶点但只有树必须的n-1条边，如果加了任意一条边就会产生环
* 度、入度、出度：无向图边的数目称为度，有向图是入度加出度。有向图所有顶点的入度等于所有顶点的出度等于边数。所有顶点的度数之和为偶数
* 权和网：边带上的数值就是权。带权图是网
* 邻接点：一条边的两个顶点相邻接/互为邻接点，边**依附于**那两个顶点
* 稀疏图和稠密图：没有准确定义，一般来说边数小于vlogv就是稀疏图
* 路径、路径长度、带权路径长度、距离：路径是两个顶点之间（含）的**顶点序列**，路径长度是路径的**边的数目**。最短路径长度称作距离，如果不存在路径，距离为无穷
* 回路/环：第一个顶点和最后一个顶点相同的**路径**。n个顶点的图有大于n-1条边则一定有环。若有向图中存在拓扑序列，则该图不存在回路
* 简单路径、简单回路：路径中的顶点不重复叫简单路径；除了最开始那个以外，顶点不重复，叫简单回路
* 有向树和有向森林：有一个顶点的入度为0，其余顶点入度为1的有向图称为有向树
* 一个无向图是树的条件：必须是无回路的连通图或者是有n-1条边的连通图（其实都是一个意思）

### 储存结构

* 邻接矩阵：无权图有边为1，无边为0，对角线为0；有权图有边为权值，无边为无穷（代表距离）；无向图沿对角线对称。不利于增加和删除点，空间复杂度高。一行代表那行结点的出度，一列代表那列结点的入度，矩阵平方后的某个点的值代表从i到j结点长度为2的路径有多少条
* 邻接表：表头：{data, firstarc}，边：{adjvex, info, nextarc}。不利于判断顶点之间是否有边，不便于计算顶点的度（主要是入度）
* 十字链表：用于有向图。弧：{tailvex弧尾的结点, headved弧头结点, hlink弧头相同的下一条弧, tlink弧尾相同的下一条弧, info}，顶点：{data, firstin第一个指向它的弧, firstout第一个指出它的弧}，可用于稀疏矩阵。图像上弧的前两个参数一般直接用顶点的索引，顶点可用顺序储存
* 邻接多重表：适用于无向图
* 对于一个图，邻接矩阵的表示法是唯一的，但邻接表不唯一，会根据输入顺序变化

### 遍历

* 非连通图无法一次就遍历完，只能访问包含那个顶点的连通分量。可以根据visited不重复地多次遍历，遍历次数就等于连通分量数
* 需要一个数组标识visited[]某个顶点是否访问过，长度与顶点数量相同
* 访问自己-\>标记-\>递归访问其他未被标记的顶点
* 深度优先DFS、广度优先BFS：基本都一样，前者用栈或者递归，后者用队列。广度优先对应二叉树的层次遍历
* 空间复杂度都为O(|V|)。时间复杂度，邻接表法都为O(V+E)，邻接矩阵都为O(V\^2)
* BFS其实能解决单源最短路径，因为它总是按照距离由近到远遍历图。但要边的权值相等
* 两种遍历都有生成树（非二叉树），但只有连通图才不会是森林

### 最小生成树

* 生成树是n-1条边，最小指权值之和最小且唯一。如果每条边的权值不同，那最小生成树就是唯一的
* Prim算法：以一个顶点开始，重复在**未选定**且可以直接与**已选定的点集连接**的**点集**中选择最小的边和点。直到树包含全部顶点
* Kruskal算法：最初把所有顶点看作一个单独的树。按权值从小到大排列边，如果选择了某条边以后可以减少一个非连通分量（不构成回路，或边的两个顶点属于不同的连通分量），则选择那条边以及两个顶点，否则不选。直到没有不连通分量

### 最短路径

出现了有冲突的题目：最短路径一定是简单路径、最短路径允许有环。虽然后者只有走过那个环后距离不变才行，如果为负就能反复走了，如果为正就不能走，为零可走可不走。

#### 迪杰斯特拉算法

1. 用于计算单源最短路径，如果要求其他点，每个都要循环一遍。时间复杂度为O(|V|\^2)。边带有负权值时不能用。
2. （手算）写出图的邻接矩阵（二维数组），方便使用“跳板”时计算距离。
3. （可选）一维数组path[]存放（源点到）顶点i的（最短路径的）前驱，结束时可根据它追溯到源点求出最短路径。如果只求最短距离就不用这个。
4. 一维数组dist[]存放源点到其他点的距离/权值。初始化时只有与源点**直接相连**的点有值，不相连的为无穷。
5. 剩余顶点集合。也可以用一个visited[]标记是否已求出最短路径。

操作步骤：

1. 初始化。
2. dist[]中**未被求出的最小的一项**即为源点到那一项的最短路径。
3. 以(2)中找到的那个点为“跳板”，更新源点到其他点的距离。即如果dist[k]\>dist[j]+arc[j][k]，就赋值过去，且path[j]=i
4. 重复(2)、(3)。

#### 弗洛伊德算法

1. 二维矩阵D[i][j]保存顶点i到j的距离，状态-1时是直接距离。（没看懂）
2. （可选）二维矩阵Path[i][j]保存j的最短前驱。当Path[x][y]=x时就表示顶点x到y直接连接；Path[x][y]=z时，表示z到y更短，此时再去找Path[x][z]是否为x；Path[i][j]=-1表示不相连。如果只求最短距离就不用。
3. A(-1)[i][j]=arcs[i][j]；A(k)[i][j]=Min{A(k-1)[i][j], A(k-1)[i][k]+A(k-1)[k][j]}。用人话说就是i和j是图的每行每列，每轮都**全部更**新一遍。从V0开始每一轮加入一个新的顶点作为跳板比较哪个小。
4. 用于解决每对顶点之间的最短路径。时间复杂度为O(|V|\^3)，允许带有负权值的边，也适用于带权无向图（看作有往返二重边的有向图）；但不允许有带负权值的边组成的回路。

### 拓扑排序（AOV网）

用于研究“先修课程”之类的问题，适用于有向无环图(DAG)。结果不唯一。

1. 在有向图中选一个无前驱的顶点并输出它。
2. 从图中删除该顶点和所有以它结尾的弧。
3. 重复(1)、(2)，直至图不存在无前驱的顶点。此时除第四点外图为空。
4. 如果此时输出的顶点数小于有向图中的顶点数（图不为空），说明存在环。

按照拓扑排序的结果生成的邻接矩阵是三角矩阵；如果一个图的邻接矩阵是三角矩阵，则存在拓扑序列。

### 关键路径（AOE网）

#### 概念

* 用于估算完成整项工程至少需要多少时间、判断哪些活动是影响工程进度的关键。顶点称为事件，边称为活动。
* 网中只有一个入度为零的点，称为源点；一个出度为零的点，称为汇点
* 找一条从源点到汇点的带权路径最长的路径，称为关键路径；关键路径上的活动称为关键活动
* 满足AOV网且带权的就是AOE网

#### 求解方式

1. 求出每个**事件**的最早发生时间ve(i)=Max{ve(k) + w(k,i)}，是带权路径最长的路径。因为只有某个顶点所有的前继结点都完成时，此顶点的工作才能开始进行，所以最早发生时间是前面的最大值。
2. 根据(1)，从后往前求出每个**事件**的最迟发生时间vl(i)=Min{vl(k) - w(i,k)}：在所有后继事件的最迟发生时间减去路径长度的结果中选择最小值。
3. 计算各**活动**的最早开始时间e(i)：活动ai=\<Vj,Vk\>的最早开始时间e(i)=ve(j)，即边的起始顶点的最早发生时间。
4. 计算各**活动**的最晚开始时间l(i)：活动ai=\<Vj,Vk\>的最晚开始时间l(i)=vl(k) - w(j,k)，即边的头结点的最晚发生时间减去边的时间；不是起始顶点的最晚发生时间。
5. 找出e(i) = l(i)的活动ai，这些边即为关键活动，由关键活动形成的从源点到汇点的每一条路径就是关键路径。关键路径可以不止一条。
6. 做完1和2已经可以看出“关键事件”了。

查找
----

* 适合静态查找：顺序查找、折半查找/二分查找、散列查找
* 适合动态查找：BST、散列查找
* 分块查找：顺序和折半的结合
* 平均查找长度ASL：所有关键字比较次数的平均值，或等于查找第i个元素的概率乘以找到第i个元素所需进行的比较次数的积的和；分为成功和不成功的
* 一般的顺序查找：ASL成功=(n+1)/2，ASL失败=n+1；有序表的顺序查找：用的是判定树，若有n个查找成功结点，则必相应有n+1个查找失败结点，**ASL成功一样**，ASL失败=n/2+n/(n+1)；这两种链式也都可以用
* 折半查找：严用的是闭区间，条件为low\<=high；ASL成功约等于log(n+1)-1；失败最好还是画图
* 失败ASL的求解方法：画出方形的虚构失败结点，**但到达它的查找长度等于到实际存在的（虚构节点的）那个父结点的长度**，只是加的次数和最后除以的次数按失败结点的数量来

### B-树/B+树

* 定义：所有孩子结点中分支最大值称为阶；关键字：大于左孩子，小于右孩子；每个结点至多含有m-1个关键字，m个孩子；若根结点不是终端结点，则至少有两棵子树；除根结点以外的所有非叶结点至少有m/2向上取整棵子树；所有叶结点在同一高度上且不带信息，类似于查找失败的结点，所有结点的平衡因子都为0
* 查找：略
* 插入：最初一定是插入到最底层的某个非叶结点内，如果该结点的关键字个数大于m-1，必须进行分裂，把中间部分的移到父结点中，递归进行
* 删除：不在底层：如果在中间，合并中间的孩子，否则就把前面/后面的一个子节点提到自己这一层，然后递归删除；在底层：大于m/2就直接删，否则看兄弟够不够借，够就把兄弟提到父关键字，原父关键字往下移；不够就直接把父结点往下移
* B+树：子树个数与关键字个数相等，关键字的值等于孩子中最大的；叶结点包含信息，非叶结点仅起到索引作用；所有相邻叶结点链接到下一个；通常有两个指针，一个指向根结点，另一个指向关键字最小的叶结点

### 散列表

* 散列函数和散列地址：直接定址法: H(key)=a\*key+b；除留余数法: H(key)=key%p；数字分析法、平方取中法、折叠法。其中p不必与数组长度相等，可以小一些，给冲突留余地
* 冲突处理方法：1.开放定址法: 线性探测法（找下一个）、二次探测法/平方探测法（依次找两边的1、4、9...位置上）、再散列法（第一个发生冲突时利用第二个散列函数计算增量）、伪随机序列法。2.链地址法（把具有相同散列地址的记录(同义词)放在单链表中）
* 在开放定址的情形下，不能随便删除表中已有元素，只能给它做个标记表示逻辑删除，因为找到空的地方就算查找失败了。因此需要定期维护
* 把不同关键字映射到同一地址称作冲突，这些关键字称为同义词。不正确的处理冲突的方法会导致聚集，聚集会影响ASL
* 平均查找长度ASL：成功、失败。前者是依次查找输入了的数据计算查找次数/元素个数。而后者是根据散列函数MOD后面的数字来计算ASL，例如MOD7，就分别看H(key)为0到6时（算一次）再散列到空时要比较的次数，再除以7
* 装填因子α：表中记录数/散列表长度，显然越小越好

排序
----

* 插入类：直接插入、折半插入、希尔排序
* 交换类：冒泡排序、快速排序：快排平均需要O(logn)的递归栈空间，时间复杂度最坏O(n\^2)，如果用严的方法就发生在基本有序的时候
* 选择类：选择排序、堆排序：都不稳定，都与初始序列状态无关
* 归并类：归并排序，时间复杂度总是O(nlogn)，空间O(n)，稳定
* 分配类：基数排序，不是基于比较，而是用多关键字排序的思想；不能用于小数；空间复杂度O(r)，时间复杂度O(n+r)
* 外部排序：多路平衡归并、置换-选择排序、最佳归并树
* 排序算法的对比：时间复杂度（最好、最坏、平均）、空间复杂度、稳定性；其它影响因素：待排序的元素数量、元素本身的信息量
* 直接插入和冒泡在排序好时复杂度都为O(n)且不需要交换；选择排序三种时间复杂度都为O(n\^2)，但如果元素本身大，更少的交换次数是优势；直接插入和选择排序的排序趟数都为n-1，与原始序列状态无关，而冒泡有关；直接插入的元素移动次数与序列状态有关，冒泡也有关
* 将两个长度为N的有序表合并，最少的比较次数是N，此时一个表的最小元素比另一个表的最大元素还大；最大的比较次数是2N-1，此时两个表中的元素是依次间隔地比较
* 任何基于比较的排序方法至少需要O(nlogn)的时间

### 希尔排序

以49, 38, 65, 97, 76, 13, 27, 48, 55为例子。若取d1=4，则所有间隔为4的记录在同一组：(49, 76, 55)、(38, 13)、(65, 27)、(97, 48)，它们内部排序。第二趟取d2\<d1，重复上面的排序，直到d=1。此时变为普通的插入排序，但之前的操作加快了最后一次排序的速度。仅适用于顺序表。

### 堆和堆排序

* 在序列对应的树中，当每个元素都大于它左右两边的元素时，它就是一个大顶堆；或者每个元素都小于它左右两边的元素时，它就是一个小顶堆。堆是一个完全二叉树。既是BST又是堆的树只有根节点和根节点加左孩子两种
* 向下调整(AdjustDown/sink)的操作：查看自己是否比两个孩子都大，如果不是，把自己与更大的那个交换。并继续对交换后的自己向下调整（因为调整后子堆也可能破坏了）；做一次的时间复杂度是O(h)即O(logn)
* 向上调整(AdjustUp/swim)：若自己大于父节点，把父节点与自己互换。并继续对交换后的自己向上调整
* 在已经是堆的序列里插入一个元素，是直接把它添加到最后一项，然后向上调整；删除元素，将堆的最后一个元素和堆顶交换，堆的大小减一，然后对堆顶向下调整
* 初建堆的操作：从n/2往前对每个元素向下调整。因为n/2之后是叶子结点，已经算合法的堆了。建堆的复杂度是O(n)
* 堆排序算法：先建堆，然后和删除元素一样。大顶堆排序后是从小到大的。不稳定。只会用到向下调整。所有的时间复杂度都是O(nlogn)，空间O(1)
* 如果只想得到序列中k个最小元素（Top K），就可以使用堆排序；不过如果是二分就要用快排的思想了
* 堆是用来排序的，在查找时它是无序的

