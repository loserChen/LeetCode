## BFS

### 模版1

```
/**
 * Return the length of the shortest path between root and target node.
 */
int BFS(Node root, Node target) {
    Queue<Node> queue;  // store all nodes which are waiting to be processed
    int step = 0;       // number of steps neeeded from root to current node
    // initialize
    add root to queue;
    // BFS
    while (queue is not empty) {
        step = step + 1;
        // iterate the nodes which are already in the queue
        int size = queue.size();
        for (int i = 0; i < size; ++i) {
            Node cur = the first node in queue;
            return step if cur is target;
            for (Node next : the neighbors of cur) {
                add next to queue;
            }
            remove the first node from queue;
        }
    }
    return -1;          // there is no path from root to target
}
```

### 模版2

有时，确保我们永远`不会访问一个结点两次`很重要。否则，我们可能陷入无限循环。如果是这样，我们可以在上面的代码中添加一个哈希集来解决这个问题。这是修改后的伪代码：

```
/**
 * Return the length of the shortest path between root and target node.
 */
int BFS(Node root, Node target) {
    Queue<Node> queue;  // store all nodes which are waiting to be processed
    Set<Node> used;     // store all the used nodes
    int step = 0;       // number of steps neeeded from root to current node
    // initialize
    add root to queue;
    add root to used;
    // BFS
    while (queue is not empty) {
        step = step + 1;
        // iterate the nodes which are already in the queue
        int size = queue.size();
        for (int i = 0; i < size; ++i) {
            Node cur = the first node in queue;
            return step if cur is target;
            for (Node next : the neighbors of cur) {
                if (next is not in used) {
                    add next to queue;
                    add next to used;
                }
            }
            remove the first node from queue;
        }
    }
    return -1;          // there is no path from root to target
}
```

### BFS练习

![220](../image/220.png)

下面是解题方法，此题我们使用刚刚学到的bfs来进行解决。主要的思路是我们首先对陆地进行遍历，找到一个root，然后利用bfs对上下左右进行搜索，由于相连的陆地只算一块陆地，则num只加一次，当遇到海水则跳出当前循环。

```
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int num = 0;
        queue<pair<int,int>> po;
        if(grid.empty()){
            return 0;
        }
        int row=grid.size();
        int col=grid[0].size();
        for(int i=0;i<row;i++){
            for(int j=0;j<col;j++){
                if(grid[i][j]=='1'){
                    po.push({i,j});
                    num+=1;
                    while(!po.empty()){
                        int x = po.front().first;
                        int y = po.front().second;
                        po.pop();
                        if(grid[x][y] == '0') //若该点为海水，跳出此次循环。进行下一次
                            continue;
                        grid[x][y] = '0';
                        bfs(grid,x+1,y,po);
                        bfs(grid,x-1,y,po);
                        bfs(grid,x,y+1,po);
                        bfs(grid,x,y-1,po);
                    }
                }
            }
        }
        return num;
    }
    void bfs(vector<vector<char>>& grid,int row,int col,queue<pair<int,int>> &po){
        if(row>=0&&row<grid.size()&&col>=0&&col<grid[0].size()){
            if(grid[row][col]=='1'){
                po.push({row,col});
            }
        }
    }
};
```

