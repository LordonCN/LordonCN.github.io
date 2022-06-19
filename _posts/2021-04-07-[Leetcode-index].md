---
layout: post
title: "[小冬找工作]-Leetcode刷题"
subtitle: '刷过的leetcode题目思路整理'
author: "Lordon"
header-img: img/jassica/jessica-jung-celebrity.jpg
catalog: true
tags:
  - [小冬找工作]
---

# LeetCode 刷题记录

本帖子分类记录刷题过程中的思路，经验以及遇到的坑。

## I. 线性表(数组+链表)

### 8M. 字符串中提取整数

**问题描述**：从一个给定的字符串中提取数字，要求从第一个非空字符开始，如果第一个非空字符不是正负符号或数字则返回0；如果超出int类型的范围（大于INT_MAX或小于INT_MIN）则返回INT_MAX或INT_MIN。

**我的思路**：很平凡的遍历思想，首先判断把空字符给跳过去，然后判断是否是数字，是数字的话记录数字。问题是坑太多，还有就是在一位一位等待数字的过程中判断是否会溢出int很重要，可以记住。

**坑**：在遍历的时候因为需要判断的条件太多，使用while循环很容易忘记判断角标i是否超出str的size。另一个坑就是判断溢出。

**优化**：判断是否溢出的代码`if((res > INT_MAX / 10) || (temp >= INT_MAX % 10 && res == INT_MAX / 10)) return INT_MAX;` 和`if((res == INT_MIN / 10 && -temp <= INT_MIN % 10) || res < INT_MIN / 10) return INT_MIN;`。

**链接**：[leetcode8](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_08m_string_stringtoint.cpp)

### 19M. 移除倒数第n个链表结点。

**问题描述**：给定链表头，移除倒数第n个链表结点。

**我的思路**：采用快慢指针，第一个指针快第二个指针n个节点遍历，当第一个指针访问到链表尾部(null)时，第二个指针刚好指向倒数第n个。

**坑：**通常删去链表结点的方法是让被删结点前一个节点指向被删结点后一个节点，但是此方法对head不起作用，需额外判断。

**链接**：[leetcode19](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_19m_list_deleteNlist.cpp)




### 21E. 合并两有序链表

**问题描述**：将两个有序链表合并成一个有序链表。

**我的思路**：思路其实很简单，从小到大两链表都有对应的指针，一个一个遍历比较大小即可，但是实现的时候把代码写的简洁却不是一件容易的事。下边放出官方给出的效率最高的代码之一。

**链接**：[leetcode21](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_21m_list_combinTwoList.cpp)


### 24M. 成对地交换链表结点

**问题描述**：给出一个链表，成对交换节点的第1、2，3、4...个节点。要求不能改变节点的值，只能改变节点的指向。

**我的思路**：很平常的思路，一道练习链表节点操作的题。

**链接**：[leetcode](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_24m_list_changeTwoNode.cpp)

### 31M. 下一种排序（Next Permutation）

**问题描述**：给出一个数字序列，找出这个数字序列的下一个排序，使得在这些数字所组成的所有序列按从小到大排列的话，新的序列刚好位于原序列的下一个。如果找不到的话，就把整个序列翻转顺序输出。

**我的思路**：在序列中找到一对数，在序列中构成升序。并且近可能位于序列尾端。再第一个数尽可能接近尾部的前提下找到最接近尾段的第二个数。将两个数交换，再把第一个数位置后边的所有数翻转。得到的序列就是答案。如果找不到这样一对数对，就把整个数组翻转。

**坑**：一开始没想到把第一个数翻转。另一个坑就是两个loop的次序一开始想错了。


**链接**：[leetcode](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_31m_vector_nextNumber.cpp)

**思考**：如果问题改成上一个排序的话，应该怎么做？同样还是找到位于最后的一对数使其组成降序，将他们交换顺序，之后再把第一个数后边的所有数翻转。

### 33M. 在旋转过的有序序列寻找目标

**问题描述**：已知一个[旋转过的有序序列]()长度为n，在log(n)时间复杂度内寻找target，存在则返回数组索引，不存在则返回-1。被旋转的序列：[0,1,2,4,5,6,7]旋转后变为[5,6,7,0,1,2,4]。

**我的思路**：首先使用二分法查找pivot，即围绕哪一点旋转的。然后再从被pivot分割成的两段中的某一段中使用二分法寻找目标。

**坑**：当数组只有一个元素时，需要特别注意。


**链接**：[leetcode](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_33m_vector_findTargetInnums.cpp)

### 34M. 在有序序列中找到元素的第一个和最后一个位置

**问题描述**：给出一个有序数组和目标元素，找出数组中该元素出现的第一个和最后一个位置。要求对数级时间复杂度。

**我的思路**：首先通过二分法，找出该元素的任意一个位置。再从该位置和二分法的左右端的最后位置分别用二分法查找该元素的左、右起始位置。

**坑**：对于常见的二分法查找，在每次迭代的过程中为了防止最后left = right + 1时的死循环，使left = middle + 1和right = middle - 1是有效的方法。但是对于寻找该元素的第一个和最后一个位置，这样的做法不合适。因为循环的终止条件并不是middle等于或不等target，而是middle等于target但是middle的下一个不是target。所以在之后的二分法中，迭代只能是left = middle 和 right = middle 而不再加减1。这样需要额外的防止死循环的方法—当middle等于left时，判断并终止。

**链接**：[leetcode](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_34m_vector_findTargetLocation.cpp)

### 56M. 融合区间

**问题描述**：给出若干个小区间，如果其中任意两个有重叠则二者可以融合，返回所有融合操作过的区间。

**我的思路**：按照区间头从小到大排序，比较好分析一些。对每对相邻区间排序，如果后者的头小于前者的尾，则可以混合，所有相邻的混合过的区间可用start和end统一表示成一个区间。这道题可分为允许和不允许对原输入进行操作的，如果允许改变输入，那么混合过后可以直接删除多余的那个，改变另一个的尾部，而剩去start和end进行计数，当然从提交结果来看，vector的erase操作是相当费时的。

**链接**：[leetcode](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_56m_vector_mergelap.cpp)


### 61M. 轮换链表

**问题描述**：给出一个链表和一个非负整数k，旋转链表，将链表每个节点向右移动k个位置，链表尾移动后变为链表头。

**我的思路**：思路可分为寻找支点(pivot)和寻找移动过后的链表头。支点即位倒数第k个数的前一个数。

**链接**：[leetcode](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_61m_list_rotateRight.cpp)

### 86M. 分割链表

**问题描述**：给定一个数组和一个分割数，输出数组满足比分割数小的数在前，其余的数在后，且与输入的相对顺序不能乱。

**我的思路**：采用双指针方法，一个指针负责遍历，另一个指针负责指向已处理好的小于分割数的节点的最前端。为了方便处理head的问题，添加了哨兵便于操作（反而使占用内存变大了）。

**链接**：[leetcode](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_86m_list_partition.cpp)


### 141E. 检测链表是否有环

参见快慢指针<https://starkschroedinger.github.io/2020/02/02/LeetCode-Notes/>。
TODO:

### 189E. 翻转数组

**问题描述**：将数组的后k位顺序不变的移动到数组的前部。

**我的思路**：在数组后边重复一个数组，同时选取符合要求的部分，重新组成数组。

**优化**：将前k位翻转，将剩下的再翻转一次，之后整个数组再翻转一次。
TODO:


### 977E. 有序数组的平方

**问题描述**：一个有序数组包含所有正负数和0，将每个数的平方输出并按大小排列成为新的有序数组，

**我的思路**：和上边一题很相似，一开始的做法是找到第一个正数，然后就和上题很相似，从左右分别检测绝对值大小并延伸下去，后来发现其实可以直接从最大的开始找，因为最大的在最左边或最右边，效率会提升很多。
TODO:


### 1365E. 数组中有多少数小于当前数

**问题描述**：给出一个数组，返回大小相同的数组，每个位置保存着原数组中有多少数小于对应位置的数。其中每个数不大于100且不小于0，总数量不大于100。

**我的思路**：建立一个大小为101的数组，每个数组都代表给定序列中这个数的个数，计数后相加。

**链接**：[leetcode]()


## II. 字符串

### 12M. 阿拉伯数字到罗马数字转换

**问题描述**：将1～3999范围内的整数转换为罗马数字。

**我的思路**：无脑转换，写的复杂一点，算起来很快。

**链接**：[leetcode](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_12m_string_intToRoman.cpp)


### 151M.翻转字符串中的单词

**问题描述**：给一字符串，将所有单词的顺序颠倒并隔空格组成新的字符串，其中单词本身不改变。所有非空格字符都看成单词的一部分，字符串收尾的空格在输出时要忽略。

**思路**：遍历后组成新的字符串，可用vector和stack来实现；也可直接在原字符串上进行更改，下边用这三种方法实现，其中因为在原字符串上涉及到很多erase操作，所有效率最低，vector效率最高。

**链接**：[leetcode](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_151m_string_reverseWord.cpp)

## III. 栈和队列

### 20E. 有效的括号对

**问题描述**：判断一个给定字符串是否是有效的括号对。有效的括号对只任何两个配对的括号对中间都必须是完整的配对括号对。如：` "{[]}"`、`"[[{}{()}]]"`等。

**我的思路**：一个自然的思路自然是stack，如果当前的是左括号就压入stack，如果是右括号就把stack顶部元素弹出，符合要求的一定会与当前括号匹配。否则返回无效。
还能够通过使用string/vector 的 erase操作成对删除符合条件的括号，while循环即可。

**坑**：注意判断stack的时候需要注意没有右括号的case，如`"(("`，在判断的时候由于没有弹出stack判断的操作自然会没有因此判断为无效，因此在末尾要判断stack内是否还有元素。

**优化**：如果本身括号的个数是奇数，那么一定不会是有效的。

**链接**：[leetcode](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_20s_stack_validkuohao.cpp)

### 84H. 直方图中最大的长方形

**问题描述**：给出一个直方图（宽为1，直方图由数组的形式给出），找出这个直方图中最大的长方形。如下图最大长方形面积即为10。

![avatar](https://assets.leetcode.com/uploads/2018/10/12/histogram_area.png)

**我的思路**：

**链接**：[leetcode](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_84h_stack_largestRectangleArea.cpp)

### 150M. 逆波兰表达式

**题目描述**：给出一个字符串序列，分别表示逆波兰表达式的各项，求出这个表达式的运算结果。

**我的思路**：首先弄清楚逆波兰表达式的原则就简单了，使用栈来保存数，遇到运算符号取栈内前两个数运算，运算结果再压入栈。

**坑**：减法和除法中减数和除数都是位于栈顶的；注意判断是否是字符时负数的符号也位于字符串首，不要误判为减号。

**链接**：[leetcode](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_150m_string_bolanfunc.cpp)



### 1019M. 链表中下一个更大的节点

**题目描述**：给定一个链表，返回一个数组，存入每个节点后第一个比他大的节点的值，如果不存在那么存入0。

**思路**：依靠单调栈的特性，我们按照上述法则依次存入节点至栈，遇到比栈顶大的，那么就在这个节点对应的数组位置处存下这个数。需要注意的是，链表并不能直接靠索引找到相应的位置，因此在未确定的时候，在数组中相应节点的位置存入的是原节点的值，这样就可以在数组中采用索引的方式加速算法。

**注**：参考单调栈 [Leetcode刷题总结 五](https://starkschroedinger.github.io/2020/03/02/Monotonous-Stack/)。

)

**链接**：[leetcode](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_1019m_list_nextbiggernumber.cpp)

## IV. 树

### 101E. 对称树

**问题描述**：检查一棵树是不是对称树。

**我的思路**：分为用队列迭代和递归的方法，前者用队列实现，后者递归的时候左树和右树传进去的子节点应刚好左右相反。

**链接**：[leetcode](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_1019m_list_nextbiggernumber.cpp)

### 102M. 二叉树的层序遍历

**问题描述**：完成对二叉树的层序遍历。

**我的思路**：一般层序遍历都是用队列实现，每次压入一层，弹出时将弹出节点的非空左右孩子再压入队列，直到队列为空，这里需要计数来区分不同层。这里也可以用迭代遍历，迭代函数中有一引用形参表示当前的层数，在对应层数的vector中压入val。

**链接**：[leetcode](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_102m_tree_levelOrder.cpp)

### 103M. 二叉树之字形层序遍历

**问题描述**：之字形层序遍历二叉树，即第一层从左往右遍历，第二层从右往左遍历。

**我的思路**：这种循环往复的自然想到用栈来实现，但是和队列有所不同的是，这里是用两个栈实现，并且从左到右和从右到左的时先压入左孩子还是先压入右孩子也不同，需要具体事例具体分析。

**链接**：[leetcode](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_103m_tree_zigzagLevelOrder.cpp)

### 104E. 二叉树的最大深度

**问题描述**：求出二叉树的最大深度

**我的思路**：递归没话说
TODO:

### 105M. 由先序遍历和中序遍历恢复二叉树

### 107M. 二叉树的层序遍历 II

**问题描述**：从最底层到最顶层层序遍历二叉树。

**我的思路**：从最顶层开始遍历然后再reverse。

**代码**：

~~~C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> res;
        if(!root) return res;
        
        queue<TreeNode*> treeQ;
        treeQ.push(root);
        
        while(!treeQ.empty())
        {
            vector<int> cur;
            int sizeQ = treeQ.size();
            for(int i = 0; i < sizeQ; i++)
            {
                TreeNode* nD = treeQ.front();
                treeQ.pop();
                cur.push_back(nD->val);
                if(nD->left) treeQ.push(nD->left);
                if(nD->right) treeQ.push(nD->right);
            }
            res.push_back(cur);
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
~~~

### 108E. 将有序链表转化为二叉搜索树

**问题描述**：将有序链表转化为二叉搜索树，要求树是平衡的。

**我的思路**：因为平衡二叉树的左右子树深度相差最多为1，所以从中间开始加入树，然后将原列表分成两半，分别对应左右子树进行迭代。

**代码**：

~~~C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        if(nums.size() == 0) return NULL;
        TreeNode* root;
        return helper(nums, 0, nums.size() - 1, root);
    }
    TreeNode* helper(vector<int>& nums, int left, int right, TreeNode* current)
    {
        
        if(left > right) return NULL;
        if(left == right) return new TreeNode(nums[left]);
        
        int middle = (left + right) / 2;
        current = new TreeNode(nums[middle]);
        current->left = helper(nums, left, middle - 1, current->left);
        current->right = helper(nums, middle + 1, right, current->right);
        return current;
    }
};
~~~

### 110E. 平衡二叉树

**问题描述**：检查一棵树是不是平衡二叉树。

**我的思路**：形参返回左该树的高度，然后一层一层比较，遇到不一样的返回false。

**代码**：

~~~C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        return search(root)>=0;
    }
    
    int search(TreeNode* root){
        if (root==NULL) return 0;
        
        int left = search(root->left);
        int right = search(root->right);
        
        if (right<0 || left<0 || abs(right-left)>1) return -1;
        else return max(left,right)+1;
    }
};
~~~

### 111E. 二叉树的最小深度

问题描述：返回二叉树的最小深度。

我的思路：递归，每次比较左子树和右子树的最小深度，遇到叶子返回。

代码：

~~~C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

class Solution {
public:
    int minDepth(TreeNode* root) {
        if(!root)
            return 0;
        if(!root->left)
            return 1 + minDepth(root->right);
        if(!root->right)
            return 1 + minDepth(root->left);
        
        return 1 + min(minDepth(root->left), minDepth(root->right));
    }
};
~~~

### 112E. 路径之和

**问题描述**：给出一个二叉树和一个target，问是否有一个从根到叶子的路径，使得路径上叶子的node之和等于target。

**我的思路**：递归，递归参数中有一个当前之和是多少，遇到叶子检查是否等于target，如果是的就返回true，否则返回到上个节点重新迭代。

坑：注意因为树中节点可能含有负数，所以并不能用当前和与

**代码**：

~~~C++
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if (root == NULL) {
            return false;
        }
        return helper2(root, sum);
    }
    
    bool helper2(TreeNode* root, int sum) {
        // std::cout << root->val << std::endl;

        if (root->right == NULL && root->left == NULL) {
            return (sum - root->val) == 0;
        }
        if (root->right == NULL) {
            return helper2(root->left, sum - root->val);
        }
        if (root->left == NULL) {
            return helper2(root->right, sum - root->val);
        }
        return helper2(root->right, sum - root->val) || helper2(root->left, sum - root->val);
    }
~~~

### 113M. 路径之和 II

问题描述：同112E，只不过这道题目要求将所有满足条件的路径找出并输出。



### 114M. 展开树为链表

**问题描述**：给出一棵二叉树，将它展成如下左子树均为空的“链表”。

~~~
    1
   / \
  2   5
 / \   \
3   4   6
~~~

将上树展成如下形式：

~~~
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
~~~

**我的思路**：如何选择合适的递归函数是关键，假设如果对于一棵树的左子树和右子树都成了链表形式，那么只需要找到左子树最右下的节点，将这个节点的下一个指向root的右节点，再将root的右节点指向root的左节点，root的左节点置空，就成功地将左子树插入到了右子树当中且不破坏形式，需要注意的是对左右节点为空的情况的判断。

**代码**：

~~~C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    void flatten(TreeNode* root) {
        recurHelper(root);
        return;
    }
    
private:
    void recurHelper(TreeNode* cur)
    {            
        if(!cur) return;
        recurHelper(cur->right);
        recurHelper(cur->left);
        TreeNode* last = cur->left;
      	//如果左子树为空
        if(!last) return;
      	//寻找左子树的最右下节点
        while(last->right)
            last = last->right;
      	//将左子树插入到root和右子树中间
        last->right = cur->right;
        cur->right = cur->left;
        cur->left = NULL;
        return;
    }
};
~~~

## V. 排序

### 75M. 颜色排序

**问题描述**：一个数组只含有0，1和2，分别代表三种颜色，给这个数组排序。要求仅一次遍历完成排序。

**我的思路**：从头开始遍历，使用三指针，一个指针用于遍历，两个指针分别位于首尾，中间指针每指向一个，就将其归位。每归位一个，相应的前或后指针就像相应的前方移动一位。

**代码**：

~~~C++
class Solution {
public:
    void swap(vector<int>& nums, int first, int second)
    {
        int temp = nums[first];
        nums[first] = nums[second];
        nums[second] = temp;
    }
    void sortColors(vector<int>& nums) {
        if(nums.empty()) return;
        int zero = 0, one = 0, two = nums.size() - 1;
        //因为这里的two和zero每次交换后都会向后顺延，所以two和zero指向的都是待处理的数据，而非处理过后的。所以one == two的情况是一定要考虑的，否则可能会少处理最后一个数据。
        while(one <= two)
        {
            if(nums[one] == 0)
            {
                //交换结果：one处可能为1和0,zero处一定是0
                swap(nums, one, zero);
                one++;
                zero++;
            }
            else if(nums[one] == 1)
            {
                one++;
            }
            else
            {
                //交换结果，one处可能为0，1和2，所以不再one++，但two处一定是2
                swap(nums, one, two);
                two--;
            }
        }
        
        return;
    }
};
~~~

### 1366M. 计数投票给team排序 周赛178

**问题描述**：n a special ranking system, each voter gives a rank from highest to lowest to all teams participated in the competition. The ordering of teams is decided by who received the most position-one votes. If two or more teams tie in the first position, we consider the second position to resolve the conflict, if they tie again, we continue this process until the ties are resolved. If two or more teams are still tied after considering all positions, we rank them alphabetically based on their team letter. Given an array of strings `votes` which is the votes of all voters in the ranking systems. Sort all teams according to the ranking system described above.

Return *a string of all teams* **sorted** by the ranking system.

Example:

~~~
Input: votes = ["ABC","ACB","ABC","ACB","ACB"]
Output: "ACB"
Explanation: Team A was ranked first place by 5 voters. No other team was voted as first place so team A is the first team.
Team B was ranked second by 2 voters and was ranked third by 3 voters.
Team C was ranked second by 3 voters and was ranked third by 2 voters.
As most of the voters ranked C second, team C is the second team and team B is the third.
~~~

**我的思路**：给每一支队伍创建一个特殊的记分数据结构，如果有n支队伍参与排名，这个积分结构则是大小为n+1的`vector<int>`,其中前n项的第i项为这个队伍被当作第i名投了多少票，最后一项是字母顺序，a是0，z是25。给这个vector重新拍一下序，利用`<algorithm>`头文件中的sort函数的第三项自定义排序函数，两个队伍在比较时从第一项开始比，如果前n项都一样则比较最后一项字母大小，自动完成排序。

**代码**：

~~~C++
class Solution {
public:
    string rankTeams(vector<string>& votes) {
        if(votes.size() == 1) return votes[0];
        int num = votes[0].size();
        unordered_map<char, int> hash;
        //在统计的最后加上他们的ASCII码
        for(int i = 0; i < num; i++)
        {
            hash[votes[0][i]] = i;
        }
        vector<int> single(num, 0);
        vector<vector<int>> all(num, single);
        for(int i = 0; i < votes.size(); i++)
        {
            for(int j = 0; j < num; j++)
            {
                char cur = votes[i][j];
                all[hash[cur]][j]++;
            }
        }
        for(int i = 0; i < num; i++)
        {
            all[i].push_back((int)(votes[0][i] - 'A'));
        }
        //排序
        sort(all.begin(), all.end(), voteTurn);
        string res = "";
        for(int i = 0; i < num; i++)
        {
            char cur = all[i].back() + 'A';
            string s(1, cur);
            res += s;
        }
        return res;
    }
    
private:
    static bool voteTurn(const vector<int>& a, const vector<int>& b)
    {
        for(int i = 0; i < a.size() - 1; i++)
        {
            if(a[i] > b[i]) return true;
            else if(a[i] < b[i]) return false;
            else continue;
        }
        if(a.back() < b.back())
            return true;
        else return false;
    }
};
~~~

## VI. 查找

### 2M. 两数相加。

**问题描述**：给定两已知链表由低到高保存两数的各位，将两数相加后返回一链表。

~~~
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
~~~

**我的思路**：由于不知道哪个链表长，所以两个链表都被在将相加后的结果所替换。为了处理最后一位是否要进位的问题，添加哨兵辅助链表的遍历。（在所有需要对链表后续进行操作的问题都建议添加哨兵并从哨兵开始用do...while{}语句）

**坑**：不用哨兵的话代码会冗杂一些。

**代码**：

~~~C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        if(!l1) return l2;
        if(!l2) return l1;
        ListNode *pt1 = l1, *pt2 = l2;
        bool flag = false;
        while(pt1 && pt2)
        {
            pt2->val += pt1->val;
            pt1->val = pt2->val;
            pt2 = pt2->next;
            pt1 = pt1->next;
        }
        ListNode *pt = new ListNode(0), *l = nullptr; //pt是哨兵
        if(pt1) 
        {
            pt->next = l1;
            l = l1;
        }
        else 
        {
            pt->next = l2;
            l = l2;
        }
        do
        {
            pt = pt->next;
            if(flag) 
            {
                pt->val++;
                flag = false;
            }
            if(pt->val >= 10)
            {
                pt->val -= 10;
                flag = true;
            }
        }while(pt->next);
        if(flag)
        {
            pt->next = new ListNode(1);
        }
        return l;
    }
};
~~~

### 15M. 三数之和

**问题描述**：给定n个整数，返回所有满足条件的三元组(a, b, c)，使得a + b + c = 0，并且这三元组不能重复。

**我的思路**：

回忆2Sum问题，用了O(n)的时间和空间，使用哈希表，将每一个已经遍历了的数对应的“余数”存入哈希表，看接下来的数是否满足条件，思想是用空间换时间。另一种方法是首先将给定数组排序（最快的快排也需要O(nlogn)的时间，因此对于2Sum并不是很划算），然后用双指针分别指向首尾，求和，根据求和的结果和target进行比较来相应地移动首或尾指针，直到求和结果等于target。

将3Sum的第一个数固定，然后问题就变成了2Sum的问题，此时target就是-a，我们可以在2Sum算法的基础上额外增加一次遍历，使得算法的复杂度处于O(n^2)的水平。剩下还需要解决一些细枝末节的问题。

**坑**:

* 为了排除因为第一个数字重复而导致的三元组重复，需要在第一个数字遍历时，遇到重复的直接跳过本次循环。

  `if(i > 0 && nums[i] == nums[i - 1]) continue;`

* 2Sum使用哈希表的思路在3Sum问题中当输入数组特别大的时候会导致Time Limit Exceeded。Test case: <https://leetcode.com/submissions/detail/300659782/testcase/>。

**加速**:

* 排序过后，当第一个数是正数时，后边两个数也都是正数，那么绝不可能相加为0，大循环可终止。

  `for(int i = 0; i < nums.size() - 2 && nums[i] <= 0; i++)`

**代码**

~~~C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        if(nums.size() < 3) return {};
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());    //为了排除重复三元组
        for(int i = 0; i < nums.size() - 2 && nums[i] <= 0; i++)
        {
            if(i > 0 && nums[i] == nums[i - 1]) continue;
            int j = i + 1, k = nums.size() - 1;
            while(j < k)
            {
                int sum = nums[i] + nums[j] + nums[k];
                if(sum < 0) j++;
                else if(sum > 0) k--;
              	//防止第二个和第三个数重复，下边的原则是，第二个数碰到一连串一样的时候，跳到最右边的那个压入res，而第三个数碰到一连串一样的时候，跳到最左边的那个。但是防止j和k是紧邻的，也就是说nums[j]和nums[k]一样，这时还是要压入res的。
                else if(nums[j] == nums[j + 1] && j != k - 1) j++;
                else if(nums[k] == nums[k - 1] && j != k - 1) k--;
                else
                {
                    res.push_back({nums[i], nums[j], nums[k]});
                    j++;
                    k--;
                }
            }
        }
        return res;
    }  
};

//使用hash表，以下算法会在某些test case运算超时。
/*
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        if(nums.size() < 3) return {};
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());    //为了排除重复三元组
        for(int i = 0; i < nums.size() - 2 && nums[i] < 0; i++)
        {
            if(i > 0 && nums[i] == nums[i - 1]) continue;
            unordered_map<int, int> hash;
            for(int j = i + 1; j < nums.size(); j++)
            {
                if(nums[j] > 0)
                {
                    auto it = hash.find(nums[j]);
                    if(it != hash.end())
                    {
                        if(!res.empty() && nums[i] == res.back()[0] && nums[j] == res.back()[2])
                        continue;
                        else res.push_back({nums[i], nums[it->second], nums[j]});
                    }
                }
                hash.insert((pair<int, int>(0 - nums[i] - nums[j], j)));
            }
        }
        return res;
    }  
};

*/
~~~

### 16M. 最接近的三数之和

**问题描述**：给出一个数列和一个目标target，找出其中的三数，这三数之和最接近target，返回这三数之和。

**我的思路**：这道题看似和15M的三数之和很相似，但有本质的区别，实际上要更简单一些，因为遇到相等的就直接返回了。大循环第一个数，从开始到允许访问的结束，之后两个从两端到中间收缩，根据当前和与target的大小关系移动第二个数和第三个数，每次计算最新的和与target之差和最小的差比较，并更新，遇到和target一样的直接返回target，如果没有最后再返回最新的和。

**代码**：

~~~C++
//代码是leetcode给出的0ms的答案，思路和我的是一样的。
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        if (nums.size() < 3) return {};
        sort(nums.begin(), nums.end());      
        int n=nums.size();
        int j, k, sum;
        int t=nums[0]+nums[1]+nums[2];
        int u=nums[n-1]+nums[n-2]+nums[n-3];
        int clsum=(abs(target-t)>abs(target-u))?t:u;
        for (int i = 0; i < nums.size()-1; i++){
            j = i+1;
            k = nums.size()-1;
            sum = -1;
            while (j < k){
                sum = nums[i] + nums[j] + nums[k];
                if(abs(sum-target)<abs(clsum-target))clsum=sum;
                if(sum-target>0)k--;
                else if(sum-target<0)j++;
                else
                    return clsum;
            }
       }
        return clsum; 
    }
};
~~~



### 18M. 四数之和

**问题描述**：给出一个数列和一个目标target，找出其中所有的四元数组，其四数之和等于target。

**我的思路**：这道题可以在三数之和的基础上，增加一轮循环，重点在于如何加速循环。首先对数组进行排序，在最外层循环，循环不用持续到最后，当第一个数大于target的四分之一时，就停止循环，因为剩下的四数之和只能大于target；同理对第二个数，当第一个数和第二个数之和大于target的一半时，停止循环；第三个数和第四个数从两边向中间靠拢。对于第三个数，当第三个数大于target减去第一个数和第二个数剩下的一半时，停止循环，第四个数从最大开始，小于target的四分之一时，停止循环。

**坑**：为了防止重复，计策和三数之和相同。对于第一个数和第二个数都采取如果前一个数和当前的数相同，那么跳过本次大循环。对于第三个和第四个数而言，如果找到了符合的四元数组，当第三个数发现下一个数和当前数一样时，直接跳到相同数的最后一个，第四个数与之相应的，遇到重复的直接跳到相同数的最前端，当然不要忘记第三个数和第四个数的情况，不符合上述的跳跃规则。

**代码**：

~~~C++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> res = {};
        if(nums.size() < 4) return res;
        sort(nums.begin(), nums.end());
        for(int i = 0; i <= nums.size() - 4 && nums[i] <= target / 4; i++)
        {
            if(i > 0 && nums[i] == nums[i - 1]) continue;
            int stop_j = target / 2 - nums[i];
            for(int j = i + 1; j <= nums.size() - 3 && nums[j] <= stop_j; j++)
            {
                if(j > i + 1 && nums[j] == nums[j - 1]) continue;
                int m = j + 1, n = nums.size() - 1;
                int stop_m = (target - nums[i] - nums[j]) / 2;
                int stop_n = target / 4;
                while(m < n && nums[m] <= stop_m && nums[n] >= stop_n)
                {
                    int sum = nums[m] + nums[n] + nums[i] + nums[j];
                    if(sum < target) m++;
                    else if(sum > target) n--;
                    else if(nums[m] == nums[m + 1] && m != n - 1) m++;
                    else if(nums[n] == nums[n - 1] && m != n - 1) n--;
                    else
                    {
                        vector<int> resTemp = {nums[i], nums[j], nums[m], nums[n]};
                        res.push_back(resTemp);
                        m++;
                        n--;
                    }
                }
            }
        }
        return res;
    }
};
~~~



### 49M. 同构异形字符串

**问题描述**：给出一组字符串，找出其中的同构异形字符串并输出，同构异形字符串指那些字符串字母组成和数量都相等的单排列不同的字符串。

**我的思路**：很平凡的思路是：首先对每个字符串按字母顺序先排序，然后将排序后的存入一个hash表，最后同构异形的都被存入一个hash表中。

**优化**：由于unordered_map内置的对string类型的key 计算hash值速度较慢，我们可以采用质数相乘的方法手动取hash值。方法是：从2开始，给26个字母每个字母分配一个质数，然后一个字符串的的hash值就是他们字母对应的质数的乘积。由于质数没有其他质因子，且乘法有交换律，因此同构异形字符串对应的hash值都相同。

**代码**：

1. 采用unordered_map的内置hash

   ~~~C++
   class Solution {
   public:
       vector<vector<string>> groupAnagrams(vector<string>& strs) {
           if(strs.empty()) return {};
           vector<vector<string>> res;
           unordered_map<string, vector<string>> hash;
           for(int i = 0; i < strs.size(); i++)
           {
               string temp = strs[i];
               sort(temp.begin(), temp.end());
               hash[temp].push_back(strs[i]);
           }
           for(auto it = hash.begin(); it != hash.end(); it++)
           {
               res.push_back(it->second);
           }
           return res;
       }
       
   };
   ~~~

2. 采用质数手动计算hash值

   ~~~C++
   class Solution {
   private:
       int primeNum[26];
   public:
       void initializePrime()
       {
           bool flag[150];
           for(int i = 0; i < sizeof(flag); i++)
           {
               flag[i] = true;
           }
           for(int i = 2; i < sizeof(flag); i++)
           {
               if(flag[i])
               {
                   int k = 2;
                   while(i * k < sizeof(flag))
                   {
                       flag[i * k] = false;
                       k++;
                   }
               }
           }
           int i = 0;
           int j = 2;
           while(i < 26)
           {
               while(!flag[j]) j++;
               primeNum[i++] = j++;
           }
           return;
       }
       unsigned long calcHash(const string& str)
       {
           unsigned long res = 1;
           for(int i = 0; i < str.size(); i++)
           {
               res *= primeNum[str[i] - 'a'];
           }
           return res;
       }
       vector<vector<string>> groupAnagrams(vector<string>& strs) {
           if(strs.empty()) return {};
           vector<vector<string>> res;
           unordered_map<unsigned long, vector<string>> hash;
           initializePrime();
           for(int i = 0; i < strs.size(); i++)
           {
               unsigned long cur = calcHash(strs[i]);
               auto it = hash.find(cur);
               if(it != hash.end())
               {
                   it->second.push_back(strs[i]);
               }
               else
               {
                   hash[cur] = {strs[i]};
               }
           }
           for(auto it = hash.begin(); it != hash.end(); it++)
           {
               res.push_back(it->second);
           }
           return res;
       }
       
   };
   ~~~

## VII. 策略性枚举

### 17M. 电话号码的字母组合

**问题描述**：给定一个字母串，每一个字母代表九宫格输入法上若干个字母，问可能组成的字母组合有哪些，并输出所有的字母组合。

**我的思路**：这个问题逻辑上非常简单，就是最基本的排列组合问题。但是在实施的过程中有一点难度。我的思路是采用迭代，每考虑后一个数字都在前一个数字对应字母组合的基础上再增加相应的几倍。如“23”，其中2对应的是“a, b, c”，在这之后增加3对应的“d，e，f”后变为"a, b, c, bd, be, bf, cd, ce, cf"，再把初始的字母组合后边添加变为"ad, ae, af, bd, be, bf, cd, ce, cf"。

**优化**：待定。

**代码**：

~~~C++
class Solution {
public:
	vector<vector<string>> dict = {
	{},{},
	{"a", "b", "c"}, 
	{"d", "e", "f"}, 
	{"g", "h", "i"}, 
	{"j", "k", "l"}, 
	{"m", "n", "o"}, 
	{"p", "q", "r", "s"}, 
	{"t", "u", "v"}, 
	{"w", "x", "y", "z"}
	};

    vector<string> letterCombinations(string digits) 
	{
        if(digits.empty()) return {};
        vector<string> res = {""};
        int digit = digits.size();
        for(int i = 0; i < digit; i++)
        {
            int cur = digits[i] - '0';
            if(cur < 2) return {};
            forward(res, cur);
        }
        if(res.size() < 3) return {};
        return res;
    }

    void forward(vector<string>& res, int n)
    {
        int size = res.size();
        for(int i = 1; i < dict[n].size(); i++)
        {
            for(int j = 0; j < size; j++)
            {
                res.push_back(res[j] + dict[n][i]);
            }
        }
        for(int j = 0; j < size; j++)
        {
            res[j] += dict[n][0];
        }
        return;
    }
    
};
~~~

### 46M. 排列组合

**问题描述**：给出一个字母彼此互不相同的字符串，输出这些字母的所有可能的排列方式。

**我的思路**：考虑递推，假设前边n个字母的所有排列方式都已经举出，那么对于每一种**排列方式**，第n+1个字母都有n+1个位置放置（每两个字母之间以及第一个字母之前和最后一个字母之后），依此递推。

**坑**：

**代码**；

~~~C++
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> res = {};
        if(nums.size() == 0) return res;
        res = {{nums[0]}};
        for(int i = 1; i < nums.size(); i++)
        {
            int size = res.size();
            for(int j = 0; j < size; j++)
            {
                for(int k = 0; k < i; k++)
                {
                    vector<int> temp(res[j]);
                    temp.insert(temp.begin() + k, nums[i]);
                    res.push_back(temp);
                }
                res[j].push_back(nums[i]);
            }
        }
        return res;
    }
};
~~~

### 48M. 旋转图像

**问题描述**：给出一个方阵，方针每个数字代表图片的每个像素，将这个图片顺时针旋转90度。

**别人的思路**：在旋转的过程中，一定是四个像素对应，互相轮换。如果一个像素的行列坐标为(col, row)，那么与之对应的顺时针旋转的位置应该是(col, row)—(col, size - row)—(size - row, size - col)—(size - col, row)。将其封装为函数`rotateInGroup4`。现在只需找到哪些坐标时第一次旋转的坐标且不会重复。从第0行开始一直到第n/2行中，第i行从第i个像素一直到第size-i个像素是不重复的第一旋转像素。

**我的思路**：此题目可以充分运用swap 与 reverse两库函数解决，首先顺序遍历数组并进行对对角线两侧数值swap，然后对每一行reverse即可。

**链接**: [leetcode48](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_48m_vector_2dVectorRotate.cpp)

### 78M. 子集

**问题描述**：给出一个包含不同的元素的序列，求出他的所有子集并输出。

**我的思路**：采用推归的形式，由于包含n个元素的集合就是在包含n-1个元素的集合的所有子集上添上这个元素并和原来的子集合并，因此可以一步一步推出所有子集。
此题目与[17](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_17h_string_letterCombinations.cpp) [46](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_46m_vector_permute_quanpailie.cpp) 
[48](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_48m_vector_2dVectorRotate.cpp) 类似.

**链接**:[leetcode78](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_78m_vector_subSet.cpp)

### 264M. 丑数II

**问题描述**：丑数是质因子只有2、5和8的数，规定1是第一个丑数。求第n个丑数。

**别人的思路**：采用三指针法。因为每一个丑数都能看作从前边某个丑数与2、5或8相乘得到。因此，我们从1开始创建三个附属于丑数序列的虚序列。每个指针都指向当前2、3或5作为比较的对象，当当前对象乘以相应因子后仍小于最大的丑数时，则不断循环前进。这样不会漏掉任何一个丑数。

**我的思路**: 直接统计吧，主要是学怎么在有质因数条件下判断成立于否。

**链接**：[leetcode264](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_264m_judge_nthUglyNumber.cpp)


## VIII. 广度优先搜索
## IX. 深度优先搜索(DFS)

### 22M. 产生括号对

**问题描述**：给出应产生的括号对数n，生成所有符合条件的括号对。其中任何一对括号对中不能包含未闭合的单个括号，如`(()`是错误的，而`((()()))`等是正确的。

**思路**：这个问题属于最基本的卡特兰数问题，把每一种情况确定下来需要用到递归。我们知道左括号和右括号的初始数量都是n，每使用一个左括号或右括号都把变量left或right减去1，递归终止条件是左括号和右括号的数量都为0。
类似于DFS深度 ,46题通过判断该数字是否用过来进行添加 ,此题目通过将数字该位括号后添加了穿插这种中间操作 ,递归过程同leetcode17

**注意**： 1先添加左括号 回溯的时候清除左括号 添加右括号 .2一开始想的时候就没有搞清楚这个该怎么递归 想成了() ()() ()()() 三种情况

**链接**：[leetcode22](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_22m_vector_generateKuohao.cpp))

### 36M. 有效的数独*

**问题描述**：判定一个完成/未完成的数独是否有效。

**我的思路**：遍历三遍，分别判定数独有效的三个条件。即横竖和九宫格不出现相同的数字。对于每次遍历，都采用一组hash来保存。缺点：效率低，遍历次数多。

**链接**：[leetcode36](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_36m_vector_isValidSudoku.cpp)

**优化**：最好能一次遍历完，需要更改数据结构，使用bitmap效率会很高。

### 39M. 组合之和

**问题描述**：给出一组数据candidates，其中无重复数据，另外给出一个目标target，求所有可能组成和为target的组合，不能有重复的解，并且**candidates中每个元素都可以无限制次数地使用**。

**别人的思路**：按照深度优先搜索的思路，从后往前遍历，对每个数，判断当前remain是否为0（每次加入解集一个数，remain即减去这个数），接下来可以用两种方法实现这道题，第一种：可以分为将这个数加入当前解集和不加入当前解集，然后氛围两个支路继续递归。需要注意的是，对于加入这个数到当前解集这个递归支路，由于每个数可用的次数不限制，那么这个支路相当于又会产生若干次递归，一直加到remain小于当前这个数为止，因此递归的次数非常之多。第二种方法：是正统的深度优先搜索，对于每一次递归，如果遍历到了当前序号为i的元素，那么都从i开始依次将剩下的元素加入解集，每加入一次都将产生一次递归。

**我的思路**：从大到小遍历 做自减计算即可(不过这样写有bug)  ，后改用递归 比较好写。

**链接**：[leetcode39](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_39m_vector_combinationSum.cpp)<br>

**链接**：[leetcode39-digui](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_39m_vector_combinationSumDigui.cpp)

### 40M. 组合之和II

**问题描述**：给出一组数据candidates，其中可能有重复的数；另外给出一个目标target，求所有可能组成和为target的组合，并且不能有重复的解。

**我的思路**：按照深度优先搜索的思路，从后往前遍历，对每个数，判断当前remain是否为0（每次加入解集一个数，remain即减去这个数），可以分为将这个数加入当前解集合和不加入当前解集合，然后分为两个支路继续递归。需要注意的是如何排除重复的解：出现重复解的原因在于，给出的candidates中本身有重复的元素，那么则需要在上边两个递归前加入条件判断语句——**如果前后两个相同的元素，前者没有被加入到本次解集中，那么在前者所产生的支路，后者也不加入解集。**举例：
参考 39题 直接在39基础上修改后出现最后结果中存在重复的情况 此处添加vector清除重复元素部分

**链接**：[leetcode40-digui](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_40m_vector_combinationSum2Digui.cpp)

```c++
    // 2d清除vector中重复元素
    vector<vector<int>>::iterator it,it1;
    for (it=++result.begin(); it != result.end();)
    {
        it1 = find(result.begin(),it,*it);    //若当前位置之前存在重复元素，删除当前元素,erase返回当前元素的下一个元素指针
        if(it1 != it)
            it=result.erase(it);
        else
            it++;
    }
```


## X. 分治法

## XI. 贪心法

### 3M. 不含重复字母的最长子串

**问题描述**：寻找一个字符串中不含重复字母的最长子串

**我的思路**：由于要检测重复，所以选择hash map来实现滑动窗口。

**链接**：[leetcode03-for-for](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_03m_string_lengthOfLongestSubstring.cpp)

### 11M. 能盛最多水的容器

**问题描述**·；有若干相距为1的立起来的板子，他们的高度依次被存在给定的数组中。现在需要找到个板子，使得这两个板子之间能盛的水最多。

![avatar](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

**我的思路**：这道题如果暴力求解需要找出两两配对的情况，复杂度在O(n^2)。现在比较巧妙的方法是，首先取首尾两个板子，然后逐渐向中间移动，直到碰头，规则是：左右两边较低一侧的指针往中间移动。这样能保证最大的情况一定能被遍历到，且只用O(n)的时间。（证明略，用反证法比较容易想清楚。）

**链接**：[leetcode11-twoPointer](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_11m_vector_maxArea.cpp)

## XII. 动态规划

### 53E. 最大的子序列

**问题描述**：给出一个序列，寻找其中一个连续的子序列，使得这个子序列的所有元素和最大。

**别人的思路**：

* 动态规划：假设我们从头到尾遍历，那么对于当前的这个数有加入和不加入子序列的两种选择，如果前边的子序列的和为正数，那么我们可以选择加入这个子序列，这时我们称前面这个子序列是有贡献的，我们将前边的子序列加入candidates；反之如果前边子序列已经为负了，那么前边的子序列就可以抛弃了，我们从当前这个数再起一个新的子序列，最后在candidate不断比较的过程中选出最优解。


**我的思路**：使用贪心与动态规划 不过`动态在哪里`还不太理解

**链接**：[leetcode53](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_53e_vector_maxSubArray.cpp)

### 120M. 三角形的最短路径

**问题描述**：给出一个从上到下排列的二维数组组成的三角阵，找出一个从最上层移动到最下层的路径使得途径点的值的和最小。移动的规则是当前点只能向下或向右下移动一格。能否用O(n)的空间复杂度解决问题呢？

~~~
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
~~~

**别人的思路**：这是个动态规划问题，我们找出每一个位置对应的值，即从(i, j)这一点移动到最下层的最小路径，那么我们知道当前点只能从正上方的点和左上方的点移动过来，因此` d[i][j] = min(d[i - 1][j], d[i - 1][j - 1]) + triangle[i][j]`。为了使用最少的空间，我们只分配一个大小为n的数组，从头到尾只利用这一个数组的空间，同时利用一些临时变量做一些记录（代码中的temp用来记录被覆盖的左上方数据，t用来记录即将被覆盖的正上方数据）。

**我的思路**：递归方式实现反向中序遍历,可以看做树

**链接**：[leetcode120](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_240m_vector_searchNumberInMatrix.cpp)


### 240M. 搜索二维矩阵 II

**问题描述**：编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target 。

**我的思路**：两种方法1、判断number是否大于当前行最小值，小于当前行最大值，遍历。2、从右上角开始查找，小了就下移，大了就左移，直到结束。

**链接**：[leetcode240](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_240m_vector_searchNumberInMatrix.cpp)

### 232E. 用栈实现队列

**问题描述**：请你仅使用两个栈实现先入先出队列。队列应当支持一般队列的支持的所有操作（push、pop、peek、empty）

**我的思路**：设计辅助栈作为交换存储即可

**链接**：[leetcode232](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_232e_stack_MyQueue.cpp)

### 739M. 隔天的温度

**问题描述**：给定一个数组，将后面数天的更大温度间隔计算出来，保存到新数组返回。

**我的思路**：1、双循环遍历 2、最小栈实现

**链接**：[leetcode739](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_739m_vector_dailyTemperatures.cpp)

### 01E.两数之和 

**专注内容**: hashmap的理解与运用。

**问题描述**：给定一个数组跟目标值，判断找到两数之和为目标值的两个数下标。

**我的思路**:使用hashmap 遍历之后查找即可

**链接**：[leetcode01](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_01e_vector_twoSum.cpp)

### 128H.最大长度序列

**专注内容**: hashset的理解与运用。

**问题描述**：给定一个数组,统计数组中存在的最长连续序列的长度。

**我的思路**:使用hashset 首先将所有数值添加到set中去重，再一遍查找大小值一遍擦除set中内容即可。

**链接**：[leetcode128](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_128h_vector_longestConsecutive.cpp)

### 332M.重新安排行程

**专注内容**: vector函数的理解与运用。

**问题描述**：给定一个机票的字符串二维数组 [from, to]，子数组中的两个成员分别表示飞机出发和降落的机场地点，对该行程进行重新规划排序。所有这些机票都属于一个从 JFK（肯尼迪国际机场）出发的先生，所以该行程必须从 JFK 开始

**我的思路**:综合运用vector中的erase reverse等方法，结合stack对结果进行比较与保存。

**链接**：[leetcode332](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_332m_vector_findItinerary.cpp)

### 69E.求平方根

**专注内容**: 折半查找

**我的思路**:通过比较 number a / mid 值大小来判断是大了还是小了

**链接**：[leetcode69](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_69e_search_sqrt.cpp)

### 76H.最小子字符串

**专注内容**: string类型操作方法以及hash辅助

**问题描述**：给定一个字符串，找到包含所给目标字符最小长度的子字符串。

**我的思路**:使用hash对给定字符进行存储，当hash.find成立之后进行长度计算并截取保存，一次遍历即可完成。

**链接**：[leetcode76](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_76h_string_minsubcontinuestring.cpp)

### 167E.两数之和2

**专注内容**: 双指针

**问题描述**：给定一个数组 求满足两数之和的下标；

**我的思路**:双指针 左右移动即可。

**链接**：[leetcode167](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_167e_vector_twoSum2.cpp)

### 88E.两数组按顺序合并

**专注内容**: 双指针

**问题描述**：给定两个排好序的数组，将vector2 合并到 vector1中 仍保持增序。

**我的思路**:从后向前遍历1&2，谁大就把谁添加到1的后面，最后如果2添加完毕直接返回，1添加完毕那么还要对2遍历结束才能返回。

**链接**：[leetcode88](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_88e_vector_mergeTwoArray.cpp)


### 765H.情人节交换位置

**专注内容**: 贪婪算法(n) hash查找与更新 

**问题描述**：给定一个vector，0-1，2-3...为成对情侣，要求将他们调整到一起，左右位置不限。

**我的思路**:使用hash进行位置存储与查找，从左到右遍历遇到不符合条件的即进行交换操作，一次交换即可。

**链接**：[leetcode765](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_765h_vector_minSwapsCouples.cpp)


### 205E,242E,647E,696E.string专题 简单题

**专注内容**: string操作 简单

**链接**：[leetcode205](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_205e_string_isIsomorphic.cpp)

**链接**：[leetcode242](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_242e_string_isAnagram.cpp)

**链接**：[leetcode647](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_647m_string_countSubstringHuiwen.cpp)

**链接**：[leetcode696](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_696e_string_countBinarySubSttrings.cpp)


### 81M.旋转数组查找数字

**专注内容**: 二分查找 多条件判断

**问题描述**：给定一个旋转vector,从中查找给定的数字

**我的思路**:二分查找变形，首先进行左右区间判断，进而加强条件判断是否单调，否则向反方向移动。

**链接**：[leetcode81](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_81m_vector_searchRotaarray.cpp)


### 413M.求差为1的等差数列个数

**专注内容**: 1 动态规划  2 hash查表遍历法

**问题描述**：给定一个旋转vector,从中查找所有差为1的等差数列 最后输出个数。

**我的思路**:将所有数字存储到hash中，用来方便查找下一个数字是否存在hash.find(i) != hash.end()即可添加到temp中。
for(auto i:nums) 循环可逐个取出每一个数列的开头元素，内加一个循环即可。vector.erase()用于擦除vector，方便使用。

**链接**：[leetcode413](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_413m_DP_dengchaShuLie.cpp)


### 70M.爬台阶

**专注内容**: 动态规划  递归

**问题描述**：给定n节台阶，每次可以走一步或者两步，求一共有多少种方式可以走完这些台阶。

**我的思路**:1-递归的方法：每一层都存在由上一层或者两层得到，那么对于n层就可递归到第0层的情况，即满足n-1>=0;n-2>=0即可。
2-动态规划的方法：每一层的可能结果可由上一次或者上两层所有可能结果求和得到，意思就是对于第一层有1种结果，第二层有两种结果，若表示为`[1,2]`,
那么第三层即可由1+2得到`[1,2,1+2]`以此类推，就成了两数求和的形式。

**链接**：[leetcode70](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_70e_DP_climbstairs.cpp)

### 198M.抢劫最大化

**专注内容**: 动态规划  

**问题描述**：给定一个数组，表示每个房子的钱财数量，输出是劫匪可以最多抢劫的钱财数量。

**我的思路**:考虑动态规划的状态转移方程`dp[i] = max(dp[i-1],dp[i-2]+nums[i])`

**链接**：[leetcode198](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_198m_DP_houserobber.cpp)

### 64M.二维最短路径

**专注内容**: 动态规划

**问题描述**：给定一个数组，计算左上到右下的最短路径。

**我的思路**:考虑动态规划的状态转移方程`dp[i][j] = min(dp[i-1][j],dp[i][j-1])`

**链接**：[leetcode64](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_64m_DP_minPath.cpp)

### 516M.最长回文子序列

**专注内容**: 动态规划

**问题描述**：给定一个string，找到其中最长的回文字符序列。

**我的思路**:子序列问题本身就相对子串、子数组更困难一些，因为前者是不连续的序列，而后两者是连续的.
设定二维数组dp，根据状态转移方程：想求 `dp[i][j]` 需要知道 `dp[i+1][j-1]`，`dp[i+1][j]`，`dp[i][j-1]` 这三个位置大小关系。

**链接**：[leetcode516](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_516m_DP_longesthuiwenxulie.cpp)

### 28M.寻找最小字符串

**专注内容**: vector.find  滑动窗口

**问题描述**：给定一个string，找到其中是否存在所给的子字符串。

**我的思路**:1、首先利用vector.find函数直接定位子字符串首字符位置，然后截取对比即可。
2、滑动窗口方法通过取固定大小的一段字符出来 与目标字符串进行对比判断。

**链接**：[leetcode28](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_28m_string_findSubstring.cpp)


### 300M.最小增量序列

**专注内容**: dp

**问题描述**：给定一个string，找到其中最大的增量序列。

**我的思路**: 参考别人思路 

**链接**：[leetcode300](https://github.com/LordonCN/leetcode-prectice/tree/main/code_learning/leetcode/leetcode_300m_DP_longestincreasexulie.cpp)






