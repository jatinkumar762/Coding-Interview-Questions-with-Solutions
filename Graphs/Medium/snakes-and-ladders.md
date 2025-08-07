https://leetcode.com/problems/snakes-and-ladders/description/

### BFS 

```java
class Solution {

    public int snakesAndLadders(int[][] board) {

        int ROWS = board.length;
        int COLS = ROWS; // 1 to N^2
        int N2 = ROWS * COLS;

        Queue<int[]> queue = new LinkedList<>();

        boolean[] visited = new boolean[N2 + 1];

        queue.add(new int[] { 0, 0 });

        visited[0] = true;

        while (!queue.isEmpty()) {

            int[] current = queue.poll();

            for (int i = 1; i <= 6; i++) {

                int newPos = current[0] + i;

                if (newPos >= N2 || visited[newPos]) {
                    continue;
                }

                if (newPos == (N2 - 1)) {
                    return current[1] + 1;
                }

                visited[newPos] = true;

                int[] matrixPos = getMatrixPos(newPos, ROWS);

                if (board[matrixPos[0]][matrixPos[1]] == -1) {
                    queue.add(new int[] { newPos, current[1] + 1 });
                } else {

                    if (board[matrixPos[0]][matrixPos[1]] == N2) {
                        return current[1] + 1;
                    }

                    queue.add(new int[] { board[matrixPos[0]][matrixPos[1]] - 1, current[1] + 1 });
                }
            }

        }

        return -1;
    }

    private int[] getMatrixPos(int pos, int N) {

        int row = N - 1 - (pos / N);

        int col;
        if ((N - row) % 2 != 0) {
            col = pos % N;
        } else {
            col = N - (pos % N) - 1;
        }

        return new int[] { row, col };
    }
}
```