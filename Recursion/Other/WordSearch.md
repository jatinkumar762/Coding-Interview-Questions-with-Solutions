https://leetcode.com/problems/word-search/description/


### Approach-1 Recursion

```java
class Solution {

    int[][] dirs = { { -1, 0 }, { 1, 0 }, { 0, -1 }, { 0, 1 } };
    int m, n, len;
    boolean[][] visited;

    public boolean exist(char[][] board, String word) {

        len = word.length();

        m = board.length;
        n = board[0].length;

        visited = new boolean[m][n];

        if (m * n < len) {
            return false;
        }

        for (int r = 0; r < m; r++) {
            for (int c = 0; c < n; c++) {
                if (board[r][c] == word.charAt(0) && checkWord(r, c, board, word, 1)) {
                    return true;
                }
            }
        }

        return false;
    }

    private boolean checkWord(int row, int col, char[][] board, String word, int index) {

        if (len == index) {
            return true;
        }

        visited[row][col] = true;

        for (int[] dir : dirs) {
            int newRow = row + dir[0];
            int newCol = col + dir[1];

            if (newRow >= 0 && newRow < m && newCol >= 0 && newCol < n) {
                if (board[newRow][newCol] == word.charAt(index) && !visited[newRow][newCol]) {

                    //visited[newRow][newCol] = true;

                    if (checkWord(newRow, newCol, board, word, index + 1)) {
                        return true;
                    }

                    visited[newRow][newCol] = false;
                }

            }
        }

        return visited[row][col] = false;
    }
}
```

**Time Complexity**

$O(N⋅3^L)$ where N is the number of cells in the board and L is the length of the word to be matched.

* For the backtracking function, initially we could have at most 4 directions to explore, but further the choices are reduced into 3 (since we won't go back to where we come from).
As a result, the execution trace after the first step could be visualized as a 3-nary tree, each of the branches represent a potential exploration in the corresponding direction. Therefore, in the worst case, the total number of invocation would be the number of nodes in a full 3-nary tree, which is about $3^L$.

* We iterate through the board for backtracking, i.e. there could be N times invocation for the backtracking function in the worst case.

* As a result, overall the time complexity of the algorithm would be $O(N⋅3^L)$.


**Space Complexity**

* O(N) + O(L)
* where N: the number of cells in the board
* where L: the length of the word to be matched.


### How to reduce Space Complexity

* we make the changes in board only
* after visiting, will assign the board cell to `#`
* later again assign back original value, when backtrack

```java
class Solution {
    private char[][] board;
    private int ROWS;
    private int COLS;

    public boolean exist(char[][] board, String word) {
        this.board = board;
        this.ROWS = board.length;
        this.COLS = board[0].length;

        for (int row = 0; row < this.ROWS; ++row)
            for (int col = 0; col < this.COLS; ++col)
                if (this.backtrack(row, col, word, 0))
                    return true;
        return false;
    }

    protected boolean backtrack(int row, int col, String word, int index) {
        /* Step 1). check the bottom case. */
        if (index >= word.length())
            return true;

        /* Step 2). Check the boundaries. */
        if (row < 0 ||
                row == this.ROWS ||
                col < 0 ||
                col == this.COLS ||
                this.board[row][col] != word.charAt(index))
            return false;

        /* Step 3). explore the neighbors in DFS */
        boolean ret = false;
        // mark the path before the next exploration
        this.board[row][col] = '#';

        int[] rowOffsets = { 0, 1, 0, -1 };
        int[] colOffsets = { 1, 0, -1, 0 };
        for (int d = 0; d < 4; ++d) {
            ret = this.backtrack(
                    row + rowOffsets[d],
                    col + colOffsets[d],
                    word,
                    index + 1);
            if (ret)
                break;
        }

        /* Step 4). clean up and return the result. */
        this.board[row][col] = word.charAt(index);
        return ret;
    }
}
```

