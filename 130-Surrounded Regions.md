# 130. 被围绕的区域
给定一个二维的矩阵，包含 'X' 和 'O'（字母 O）。

找到所有被 'X' 围绕的区域，并将这些区域里所有的 'O' 用 'X' 填充。

**举例**
```
X X X X
X O O X
X X O X
X O X X
运行你的函数后，矩阵变为：
X X X X
X X X X
X X X X
X O X X
```

## 代码
```
class Solution {
public:
    void solve(vector<vector<char>>& board) {
        if(board.size() == 0 || board[0].size() == 0)
            return;

        for(int i = 0; i < board[0].size(); i++) {
            // 第一行
            if(board[0][i] == 'O')
                dfs_func(board, 0, i);

            // 最后一行
            if(board[board.size()-1][i] == 'O')
                dfs_func(board, board.size()-1, i);
        }

        for(int i = 0; i < board.size(); i++) {
            // 第一列
            if(board[i][0] == 'O')
                dfs_func(board, i, 0);
            
            // 最后一列
            if(board[i][board[0].size()-1] == 'O')
                dfs_func(board, i, board[0].size()-1);
        }

        for(int i = 0; i < board.size(); i++) {
            for(int j = 0; j < board[0].size(); j++) {
                if(board[i][j] == 'O')
                    board[i][j] = 'X';
            }
        }

        for(int i = 0; i < board.size(); i++) {
            for(int j = 0; j < board[0].size(); j++) {
                if(board[i][j] == 'Y')
                    board[i][j] = 'O';
            }
        }
    }

    void dfs_func(vector<vector<char>>& board, int i, int j) {
        if(i < 0 || i >= board.size() || j < 0 || j >= board[0].size())
            return;
        
        if(board[i][j] != 'O')
            return;
        
        board[i][j] = 'Y';
        dfs_func(board, i-1, j);
        dfs_func(board, i+1, j);
        dfs_func(board, i, j-1);
        dfs_func(board, i, j+1);
    }
};
```
