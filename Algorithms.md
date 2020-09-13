# 算法笔记

[经典总结1](https://github.com/MisterBooo/LeetCodeAnimation)

## 算法专题

- #### 动态规划

![img](https://pic.leetcode-cn.com/1f95da43d1bdeebdd1213bb804034ddc5f906dc61451cd63f2b5ab5d0eb33b33-%E3%80%8C%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E3%80%8D%E9%97%AE%E9%A2%98%E6%80%9D%E8%80%83%E6%96%B9%E5%90%91.png)

- #### 其他

## 经典问题

- #### 排序

  - 稳定性：两个相等的数,经过排序之后,其在序列的前后位置顺序不变，则是稳定的。
    - **稳定性排序**：冒泡排序，插入排序、归并排序、基数排序
    - **不稳定排序**：选择排序、快速排序、希尔排序、堆排序
  - 原地排序： 不占用额外内存资源的排序

- #### 分苹果问题

**一种是列出符合条件的解，可使用递归；**

**一种是让给出解的个数，用数学分析*。**

> 把M个同样的苹果放在N个同样的盘子里，允许有的盘子空着不放，问共有多少种不同的分法？M, N为自然数。说明：如有7个苹果，2个盘子，则(5, 1, 1)和(1, 5, 1)和(1, 1, 5)都是同一种分法。
>
> 输入：     第一行一个整数表示数据的组数（多组数据），对于每组数据第一行是苹果个数M (1 ≤ m ≤ 100) ，第二行是盘子个数N(1 ≤ n ≤ 100)。
> 输出：    每组数据输出一行,放苹果的方法个数。

```c
// M个苹果， N个盘子
int solution(int m,int n){   
    if(m==0||n==1)    
        return 1;     
    if(n>m)  
        return fun(m,m);  //如果前面的小于后面的，则一定会有空盘子，则等于m个苹果放入m个盘子 
    else  
        return fun(m,n-1)+fun(m-n,n);  //有空盘子的情况 +　没有空盘子的情况 
} 
```

- #### 斐波那契（[青蛙跳台阶](https://www.nowcoder.com/practice/8c82a5b80378478f9484d87d1c5f12a4?tpId=13&tqId=11161&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)）

**要求计算第n项，非递归的实现**

```c
    int Fibonacci(int n) {
        if (n==0)
            return 0;
        int data[3] = {1, 0, 1};
        for(int i=0; i<n-1; i++){
            data[2] = data[1] + data[0];
            data[1] = data[0];
            data[0] = data[2];
        }
        return data[2];
    }
```

- #### [二叉树重建](https://www.nowcoder.com/practice/8a19cbe657394eeaac2f6ea9b0f6fcf6?tpId=13&tqId=11157&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)（先序+中序->二叉树）

简单的递归实现，思路清晰，简单的思想就是通过建立子函数，每次传入子树的先序和中序序列，递归求解

```c++
TreeNode* reConstructBinaryTree(vector<int> pre, vector<int> vin) {
    TreeNode* root = reConstructBinaryTree(pre, 0, pre.size()-1, vin, 0, vin.size()-1);
    return root;
}
TreeNode* reConstructBinaryTree(vector<int> pre, int sp, int ep, vector<int> in, int si, int ei) {
    if(sp > ep||si > ei)
    	return NULL;
    TreeNode* root =new TreeNode(pre[sp]);

    for(int i=si; i<=ei; i++)
    if(in[i]==pre[sp]){
        root->left  = reConstructBinaryTree(pre, sp+1, sp+i-si, in, si, i-1);
        root->right = reConstructBinaryTree(pre, i-si+sp+1, ep, in, i+1, ei);
        break;
    }
	return root;
}
```
- #### 出栈顺序验证（[栈模拟](https://www.nowcoder.com/practice/d77d11405cc7470d82554cb392585106?tpId=13&tqId=11174&tPage=2&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)）

```c++
bool IsPopOrder(vector<int> pushV,vector<int> popV) {
    if(pushV.size() == 0) return false;
    vector<int> stack; int j = 0;
    for(int i=0; i<pushV.size(); i++){
        stack.push_back(pushV[i]);
        while(j < popV.size() && stack.back() == popV[j]){
            stack.pop_back();
            j++;
        }
    }
    return stack.empty();
}
```
- #### 查找二叉树转双链表([链接](https://www.nowcoder.com/practice/947f6eb80d944a84850b0538bf0ec3a5?tpId=13&tqId=11179&tPage=2&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking))

```c++
    TreeNode* pre      = NULL;
    TreeNode* lastLeft = NULL;
    TreeNode* Convert(TreeNode* pRootOfTree)
    {
        if(pRootOfTree==NULL) return NULL;
        Convert(pRootOfTree->left);
        pRootOfTree->left = pre;
        if(pre != NULL)
            pre->right = pRootOfTree;
        pre = pRootOfTree;
        lastLeft = lastLeft==NULL ? pRootOfTree : lastLeft;
        Convert(pRootOfTree->right);
        return lastLeft;
    }
```

- ##### 逆序数对

```c++
/* 归并排序：
	在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。
*/
int divide(vector<int>& nums, vector<int>& res, int head, int tail){
        if(head>=tail) return 0;
        int mid = (tail + head) / 2;
        int cnt = divide(nums, res, head, mid) + divide(nums, res, mid+1, tail);
        int i = head, j = mid+1, p = head;
        while(i<=mid && j<=tail){
            if(nums[i]<=nums[j]){
                cnt += (j - (mid+1));
                res[p++] = nums[i++];
            } else {
                res[p++] = nums[j++];
            }
        }
        for(int m=i; m<=mid; ++m){
            cnt += (j - (mid + 1));
            res[p++] = nums[m];
        }
        for(int m=j; m<=tail; ++m)
            res[p++] = nums[m];
        copy(res.begin()+head, res.begin()+tail+1, nums.begin()+head);
        return cnt;
    }
    
    int reversePairs(vector<int>& nums) {
        int n = nums.size();
        vector<int> res(n);
        return divide(nums, res,  0, n-1);
    }
```

- ##### 子串匹配

  - 最大对称子串问题
  - 最大公子串问题
  - 子串匹配问题
  - 最大公子序列问题

- #### 字典序算法

```shell
# 346987521的下一个字典序
1，从尾部往前找第一个P(i-1) < P(i)的位置
3 4 6 <- 9 <- 8 <- 7 <- 5 <- 2 <- 1
最终找到6是第一个变小的数字，记录下6的位置i-1
2，从i位置往后找到最后一个大于6的数
3 4 6 -> 9 -> 8 -> 7 5 2 1
最终找到7的位置，记录位置为m
3，交换位置i-1和m的值
3 4 7 9 8 6 5 2 1
4，倒序i位置后的所有数据
3 4 7 1 2 5 6 8 9
则347125689为346987521的下一个排列
```

- ##### 删除链表的重复节点

```c++
// 非递归方法
class Solution {
public:
    ListNode* deleteDuplication(ListNode* pHead)
    {
        if(pHead==NULL || pHead->next==NULL) return pHead;
        ListNode *Head = new ListNode(0);
        Head->next = pHead;
        ListNode *pre = Head;
        ListNode *ens = pre->next;
        while(ens!=NULL){
            if(ens->next!=NULL && ens->val == ens->next->val){
                while(ens->next!=NULL && ens->val == ens->next->val)
                    ens = ens->next;
                pre->next = ens->next;
                ens = ens->next;
            } else {
                pre = pre->next;
                ens = ens->next;
            }
        }
        return Head->next;
    }
};

//递归方法

```



- #### [搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)（字节跳动经典）

```c++
// 上升序列数组经过循环移位后，二分法查找某一元素是否存在
int search(vector<int>& nums, int target) {
        int head = 0, tail = nums.size() - 1;
        while(head<=tail){
            int mid = (head + tail) >> 1;
            if(target == nums[mid]) return mid;

            if(nums[head] <= nums[mid]){
                if(target >= nums[head] && target < nums[mid]){
                    tail = mid - 1;
                } else {
                    head = mid + 1;
                }
            } else {
                if(target > nums[mid] && target <= nums[tail]){
                    head = mid + 1;
                }else{
                    tail = mid - 1;
                }
            }
        }
        return -1;
    }
```

- #### [ 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int i = find (nums, 0, nums.size(), target + 0.5);
        if (i < 1 || nums[i - 1] != target) return 0;
        return i - find (nums, 0, i, target - 0.5);    
    }
    
    int find (vector<int>& nums, int i, int j, double target) {
        int mid;
        while (i < j) {
            mid = i + (j - i) / 2;
            if (nums[mid] < target) i = mid + 1;
            else j = mid;
        }
        
        return i;
    }
};
```



- ##### 毒药问题

- ##### 卡特兰数

- ##### 孤岛问题

## 数据结构

- #### 二叉树遍历

  > 二叉树的遍历方式分为3种：前序遍历（A->B->C）、中序遍历（B->A->C）、后序遍历（B->C->A）。
  >
  > 已知前序和中序，后序和中序遍历序列之后，可以唯一确定一棵二叉树。但是，只知道前序和后序遍历序列，是无法知道哪个结点是左子树还算右子树。
  >
  > 另外的遍历方式将遍历分为深度优先遍历（DFS）和广度优先遍历（BFS）。深度优先遍历和前序遍历相似，但左右节点上没有先后顺序，此时广度优先遍历则是分层遍历，是一种自上而下的遍历方式。

  **DFS实现：**

  数据结构：栈

  父节点入栈，父节点出栈，先右子节点入栈，后左子节点入栈。递归遍历全部节点即可

  **BFS实现：**

  数据结构：队列

  父节点入队，父节点出队列，先左子节点入队，后右子节点入队。递归遍历全部节点即可

  ![1579182355860](./pic/1579182355860.png)

  - ##### 习题1

  ![1579182850479](./pic/1579182850479.png)

  > 前序遍历：ABCDEFGHK
  >
  > 中序遍历：BDCAEHGKF
  >
  > 后序遍历：DCBHKGFEA

  - ##### 习题2




## 编程基础（C语言）

- ##### 输入函数

> **char *gets(char *str);** 
>
> 功能是从输入缓冲区中读取一个字符串存储到字符指针变量 str 所指向的内存空间。遇到回车后执行结束。
>
> 使用 gets() 时，系统会将最后“敲”的换行符从缓冲区中取出来，然后丢弃，所以缓冲区中不会遗留换行符。
>
> 它在执行前不会主动清除缓存空间，只是执行后才清缓存区，因此每次新接收变量前，执行getchar()。
>
> **int scanf(const char *format, ...);**
>
> 功能是从键盘输入的字符转化为“输入控制符”所规定格式的数据，然后存入以输入参数的值为地址的变量中。
>
> ```c
> // 读取含空格的字符串，遇到回车结束，并抛弃字符串中的'\n'
> scanf("%[^\n]%*c",str);
> ```
>
> 

作者：冠状病毒biss
链接：https://www.nowcoder.com/discuss/429605?type=0&order=7&pos=6&page=1&channel=1000&source_id=discuss_center_0
来源：牛客网





# C语言基础

- #### static

一、概述
在c语言中static恰当的使用能让程序更加完美，细节上的严谨，代码会更好，也更利于程序的维护与扩展。

而static使用灵活，且又有两种完全无关的用法，所以整理总结一下。

二、static的两种用法：
1、static修饰局部变量，成为一个局部静态变量。

2、static修饰全局变量与函数，成为静态全局变量与静态函数。

三、相关涉及概念
　　可能会疑惑，修饰全局变量与修饰函数怎么会是一个用法？

　　static涉及的东西也比较多，以下有几个概念需要明白。 

 1、什么是存储类
　　简单的说也就是存储类型，c中变量是在哪里存放的？内存是怎么管理的？

　　所以内存的管理：

①栈：局部变量，函数调用传参的过程

②堆：动态存储区，需要程序员去申请释放

③数据段(data段)：显式初始化非零的全局变量(static修饰显式初始化非0的局部变量)

④bss段：显式初始化为0与未初始化的全局变量(static修饰显式初始化为0与未初始化的局部变量)

⑤text段：代码(函数)、只读数据

 

2、什么是生命周期
描述变量什么时候诞生，什么时候消亡，从诞生到消亡就是这个变量的生命周期。

①局部变量(栈)，生命周期即是进入函数，从变量创建到函数返回时变量死亡。

②全局变量(data/bss)，生命周期是程序执行到程序结束

③堆变量，生命周期是从我们malloc到free

 

3、什么是作用域
描述变量的作用的代码范围。c的作用域规则是代码块作用域，即是一对花括号{}。

一般的从变量定义到{}结束，即是这个变量的作用域

全局变量与函数一般是文件作用域，即作用域是整个.c

 

4、什么是链接属性
编译器将很多源文件编译成很多.o文件后，每个.o文件里有符号、代码段、data/bss等等的分段，链接器需要通过符号将这些内存链接起来。而这些符号就是链接属性。

c中有三种链接属性：外链接、内链接、无链接属性。

外链接：外部链接，可以在整个程序(跨文件)链接。

　　　　普通的函数与全局变量。

内链接：内部连接，只能在当前.c文件进行链接。

　　　　static修饰的全局变量与函数

无链接：没有链接。

　　　   普通局部变量　

 

四、具体分析static的两种用法
1、static修饰局部变量(静态局部变量)与普通局部变量相比
①静态局部变量作用域与连接属性与普通局部变量一样

②存储类：静态局部变量分配在data/bss段，普通局部变量在栈上。

③生命周期：因为存储类的不同，静态局部变量的生命周期得到延长了，直到程序结束。

所以当局部静态变量离开作用域后，并没有销毁，而是仍然驻留在内存当中，只不过我们不能再对它进行访问，直到该函数再次被调用，并且值不变。

2、static修饰全局变量\函数 与 普通全局变量\普通函数相比
存储类、生命周期、作用域都一样

差别在于static修饰全局变量\函数连接属性是内连接，普通全局变量\普通函数是外链接

就是说static修饰全局变量\函数不能跨文件访问调用

 

五、补充：为什么需要这样？
原因是命名的冲突、一个大工程不是常常不是一个人完成的，难免会遇到全局变量、函数命名一样。为了减少这类冲突static是很有用的工具。但是很可惜并不能完全解决，所以我们应该有效的去避免减少此类情况的发生。

如果函数仅仅被同一个源文件调用时，我们就应该声明该函数为static。

- ####  野指针

  1. 指针变量未初始化
  2. 删除内存后没有将该指针置为NULL
  3. 指针操作超越变量作用域

  ![1591943688053](pic/1591943688053.png)

