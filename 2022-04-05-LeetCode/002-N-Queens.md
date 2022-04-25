这篇文章讨论下 LeetCode 上边儿的 **51** 号问题： N 皇后 (N Queens)

## 1 问题描述：

> n 皇后问题 研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。
>
> 给你一个整数 n ，返回所有不同的 n 皇后问题 的解决方案。
>
> 每一种解法包含一个不同的 n 皇后问题 的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。
>
> 任意两个皇后不能处于 同行、同列或者斜线直线上。

此问题要求给出所有合法的皇后摆放方式。

**举个例子**

![img](002-N-Queens.assets/queens.jpg)

可见上图，任意两个皇后之间都不在同一行上，也不在同一列上，也不在斜线上。所谓斜线就是像五子棋那样的斜线，而在 N 皇后的摆放规则中是不允许这样的。

这个例子是当 n 等于 4 的时候，这是一个 4 行 4 列的棋盘，并且这时候你拥有 4 个棋子，这 4 个棋子的摆放方式要符合刚才的三个不。

当你的程序输入的值为 n 的时候希望你给所有的摆放方式的解。

## 2 解题思路

对于这样的问题，最直接的想法就是穷举所有的可能性。也就是说，你可以尝试所有的摆放方式，

当然，你可以按照一定的顺序把你手中的 n 个棋子全都摆好之后你再看一下当前的摆放方式是否符合要求。

另外，你也可以在按照一定的次序摆放的过程中顺便检查当前落下这颗棋子之后，此时的局面是不是就已经不符合 N 皇后的要求了呐？如果这时候就已经不符合要求了，那么你就可以尝试下一种摆放方式了，因为当前已经不符合，再继续摆下去也没有意义了。

那么在这里，我才用第二种方式，就是在摆放的过程中就边摆放边验证。

**举个例子**

这里我一行一行地来摆放棋子，并且每一行都从**第 0 号**格子开始依次尝试到**第 n-1 号**格子。因为要求不同行，所以每行至多一个棋子。

假设：

我们先摆放了第 0 号行的第 0 个位置一颗棋子，

接着摆放第 1 号行的第 0 个位置一颗棋子，

这时候我们就已经没有必要去尝试摆放第 2 号行了，因为此时已经不合法了。因为我们摆放的那两颗棋子位于了同一列上，那么这时候应该直接跳过第 1 号行的那种摆放方式而继续去尝试接下来的一种摆放方式。

那么这时候撤销第 1 号行摆放的棋子，然后尝试 1 号行且 1 号列的摆放方式，这时候你会发现，当前摆放位置的左上角的格子存在棋子，所以不合法。

再撤销刚才的棋子，这次尝试 1 号行且 2 号列的位置摆放棋子，现在你会发现，已经没有冲突了，那么这时候就可以在第 2 号行尝试所有的位置摆放棋子了，若冲突则换下一个位置尝试，直到所有的行都摆放上了棋子，这时候我们就得到了一个合法的摆放方式了！

相信你理解了~

**那么，如何验证合法性？**

每当我们摆放一颗棋子之后，我们就已经检查当前摆放的位置的左上角方向上所有的格子是否存在棋子，检查当前位置的右上角方向上的所有格子是否存在棋子，最后检查一下当前位置的正上方的所有格子是否存在棋子。

那么为什么不用检查左下角方向上的所有格子、右下角方向上的所有格子、正下方的位置上所有的格子呐？答案就是：因为我们是一行一行的摆放呀，所以下边的行都还没有摆放棋子呀，所有也就用不着检查了呀！那么又为什么不用检查当前行上是否存在其他的棋子呐？因为我们每次只在一行上摆放一个棋子呀！从一开始就避免了在行方向上出现不合法的情况了。

## 3 代码实现 C++

**LeetCode 给的代码**

```c++
class Solution {
public:
    vector<vector<string>> solveNQueens(int n) {
        
    }
};
```

LeetCode 希望我们返回一个 vector<vector<string>> 类型的数据，这个类型的数据中每一个 vector<string> 记录了一个棋盘的样子。

vector<string> 相当于一个二位数组，

vector<vector<string>> 相当于一个三位数组，

意会一下，相信你理解了 ~

**算法思想**

整个算法的实现其实就是一个穷举的过程，属于 **回溯算法**。

**穷举的逻辑**

你会发现，我在此添加了两个函数，分别是 **backtrack** 和 **isValid**；

另外还有一个变量 **res**；

其中 **res** 的类型和 **solveNQueens** 函数的返回值类型一样，也就是说，我们使用 **res** 来记录所有合法的棋盘的样子，每穷举到一个合法的棋盘的时候，我们就添加到 **res** 中去。

**backtrack 回溯**

我们先来关注 **backtrack** 函数的逻辑

其他地方的代码都不要看，只看 **backtrack** 函数的代码以及它的注释

```c++
class Solution {
public:
    vector<vector<string>> res;

    vector<vector<string>> solveNQueens(int n) {

        vector<string> board(n, string(n, '.'));
        backtrack(board, 0);

        return res;
    }

    // 用于判断当我们在 row 号行 col 号列
    // 摆放一个棋子的时候，检查当前摆放是否合法
    bool isValid(vector<string>& board, int row, int col){

    }

    // backtrack 函数的定义
    // 在棋盘 board 中对 row 号行进行棋子的摆放
    void backtrack(vector<string>& board, int row){
        if(row >= board.size()){
            // 应注意到 board.size() 的大小刚好是等于 n 的
            // n 是棋盘的长宽
            // 若 row 等于 n 说明我们正在对第 n 号行摆放棋子
            // 而我们的行号从 0 开始编号
            // 那也就说明了我们已经摆放完成了 n 个棋子
            // 能够递归到这一层，说明之前的选择都是合法的
            // 则当前 board 纳入到 res 中
            res.push_back(board);
            return;
        }

        // 对于每一层都可以进行这样的选择
        for(int col = 0; col < board.size(); ++col){
            // 做出选择前，对该情况判断是否合法
            // 若不合法则直接跳过
            if(!isValid(board, row, col)) continue;

            // 做出对当前行的选择
            // 并放入到 board 中
            board[row][col] = 'Q';

            // 对下一行进行选择
            backtrack(board, row+1);

            // 撤销当前行的选择
            // 进行下一轮的循环，重新对当前行选择
            board[row][col] = '.';
        }
    }

};
```



好啦！在你认真意会了 **backtrack** 函数之后，相信你已经懂了它的逻辑。

关键问题是，现在 **isValid** 函数的逻辑还没有写，那么接下来我们重点关注一下 **isValid** 函数的逻辑。

**isValid**

我在这里为了节约篇幅，单独把 **isValid** 函数给拿出来；

```c++
// 用于判断当我们在 row 号行 col 号列
// 摆放一个棋子的时候，检查当前摆放是否合法
bool isValid(vector<string>& board, int row, int col){
        // 因为是自上而下一行一行摆放皇后
        // 所以在当前行的下方不存在皇后
        // 则只需要检查此行上方的方向即可

        // 1 检查左上方所有格子;注意对数组下标是否越界进行检查
        for(int i = row-1, j = col-1; i >= 0 && j >= 0; --i, --j){
            if(board[i][j] == 'Q')
                return false;
        }

        // 2 检查正上方所有格子;注意对数组下标是否越界进行检查
        for(int i = row-1; i >= 0 ; --i){
            if(board[i][col] == 'Q')
                return false;
        }

        // 3 检查右上方所有格子;注意对数组下标是否越界进行检查
        for(int i = row-1, j = col+1; i >= 0 && j < board.size(); --i, ++j){
            if(board[i][j] == 'Q')
                return false;
        }
		
    	// 若以上三种情况都合法，则返回 true 
        return true;

}
```



**solveNQueens** 

最后重点关注下 **solveNQueens** 函数

```c++
vector<vector<string>> solveNQueens(int n) {

    vector<string> board(n, string(n, '.'));
    backtrack(board, 0);

    return res;
}
```

这个函数就是，首先初始化一下最一开始的棋盘，因为还没有摆放任何的棋子，所以此时的棋盘的每一个位置都应该初始化为符号 **.** 一个点。

接着，我们看 **backtrack** 函数的定义：

在棋盘 board 中对 row 号行进行棋子的摆放

因为我们的行号从 0 开始，所以我们调用 **backtrack** 函数的时候应该先从第 0 号行开始，接着后边的工作，它都会自动进行下去了。

**完整代码**

```c++
// n 行 n 列， 其中 . 代表空，Q 代表皇后
// 任意两个皇后都不能同行、同列、同对角线 
// -> 共六个方向
// 输出所有合法的解
// 返回一个 vector<vector<string>> 类型的数据
class Solution {
public:
    vector<vector<string>> res;

    vector<vector<string>> solveNQueens(int n) {

        vector<string> board(n, string(n, '.'));
        backtrack(board, 0);

        return res;
    }

    bool isValid(vector<string>& board, int row, int col){
        // 因为是自上而下一行一行摆放皇后
        // 所以在当前行的下方不存在皇后
        // 则只需要检查此行上方的方向即可

        // 1 检查左上方
        for(int i = row-1, j = col-1; i >= 0 && j >= 0; --i, --j){
            if(board[i][j] == 'Q')
                return false;
        }

        // 2 检查正上方
        for(int i = row-1; i >= 0 ; --i){
            if(board[i][col] == 'Q')
                return false;
        }

        // 3 检查右上方
        for(int i = row-1, j = col+1; i >= 0 && j < board.size(); --i, ++j){
            if(board[i][j] == 'Q')
                return false;
        }

        return true;

    }

    void backtrack(vector<string>& board, int row){
        if(row >= board.size()){
            // 能够递归到这一层，说明之前的选择都是合法的
            // 则当前 board 纳入到 res 中
            res.push_back(board);
            return;
        }

        // 对于每一层都可以进行这样的选择
        for(int col = 0; col < board.size(); ++col){
            // 做出选择前，对该情况判断是否合法
            // 若不合法则直接跳过
            if(!isValid(board, row, col)) continue;

            // 做出对当前行的选择
            // 并放入到 board 中
            board[row][col] = 'Q';

            // 对下一行进行选择
            backtrack(board, row+1);

            // 撤销当前行的选择
            // 进行下一轮的循环，重新对当前行选择
            board[row][col] = '.';
        }
    }

};
```



