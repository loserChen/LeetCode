## DFS

正如我们在本章的描述中提到的，在大多数情况下，我们在能使用 BFS 时也可以使用 DFS。但是有一个重要的区别：`遍历顺序`。

与 BFS 不同，`更早访问的结点可能不是更靠近根结点的结点`。因此，你在 DFS 中找到的第一条路径`可能不是最短路径`。

在本文中，我们将为你提供一个 DFS 的递归模板，并向你展示栈是如何帮助这个过程的。在这篇文章之后，我们会提供一些练习给大家练习。

```
/*
 * Return true if there is a path from cur to target.
 */
boolean DFS(Node cur, Node target, Set<Node> visited) {
    return true if cur is target;
    for (next : each neighbor of cur) {
        if (next is not in visited) {
            add next to visted;
            return true if DFS(next, target, visited) == true;
        }
    }
    return false;
}
```

递归解决方案的优点是它更容易实现。 但是，存在一个很大的缺点：如果递归的深度太高，你将遭受堆栈溢出。 在这种情况下，您可能会希望使用 BFS，或使用显式栈实现 DFS。

这里我们提供了一个使用显式栈的模板：

```
/*
 * Return true if there is a path from cur to target.
 */
boolean DFS(int root, int target) {
    Set<Node> visited;
    Stack<Node> s;
    add root to s;
    while (s is not empty) {
        Node cur = the top element in s;
        return true if cur is target;
        for (Node next : the neighbors of cur) {
            if (next is not in visited) {
                add next to s;
                add next to visited;
            }
        }
        remove cur from s;
    }
    return false;
}
```

### 岛屿数量

给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

 

**示例 1:**

```
输入:
11110
11010
11000
00000
输出: 1
```

**示例 2:**

```
输入:
11000
11000
00100
00011
输出: 3
解释: 每座岛屿只能由水平和/或竖直方向上相邻的陆地连接而成。
```

利用dfs隐式栈来进行深搜。

```
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int num = 0;
        if(grid.empty()){
            return 0;
        }
        int row=grid.size();
        int col=grid[0].size();
        for(int i=0;i<row;i++){
            for(int j=0;j<col;j++){
                if(grid[i][j]=='1'){
                    dfs(grid,i,j);
                    num+=1;
                }
            }
        }
        return num;
    }
    void dfs(vector<vector<char>>& grid,int row,int col){
        if(row>=0&&row<grid.size()&&col>=0&&col<grid[0].size()){
            if(grid[row][col]!= '1') return;
            grid[row][col] = '0';
            dfs(grid,row+1,col);
            dfs(grid,row-1,col);
            dfs(grid,row,col+1);
            dfs(grid,row,col-1);
        }
        
    }
};
```

### 二叉树的中序遍历

给定一个二叉树，返回它的*中序* 遍历。

**示例:**

```
输入: [1,null,2,3]
   1
    \
     2
    /
   3

输出: [1,3,2]
```

递归算法遵循左中右原则，进行递归。

```
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
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> a;
        search(root,a);
        return a;
    }
    void search(TreeNode* root,vector<int> &a){
        if(root==NULL){
            return;
        }
        search(root->left,a);
        a.push_back(root->val);
        search(root->right,a);
    }
};
```

使用栈来进行迭代

```
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
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> a;
        stack<TreeNode*> s;
        TreeNode *temp =root;
        while(temp||!s.empty()){
            while(temp){
                s.push(temp);
                temp = temp->left;
            }
            temp = s.top();
            s.pop();
            a.push_back(temp -> val);
            temp = temp -> right;  
        }
        return a;
    }
};
```

