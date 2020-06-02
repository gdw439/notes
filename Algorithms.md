# 算法笔记

[经典总结1](https://github.com/MisterBooo/LeetCodeAnimation)

## 1. 经典问题

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
/* 在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。
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

- ##### 毒药问题

- ##### 卡特兰数

- ##### N皇后

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

