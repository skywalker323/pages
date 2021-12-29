# 353 Design Snake Game

### Problem:

<pre>
Design a Snake game that is played on a device with screen size = width x height. Play the game online if you are not familiar with the game.

The snake is initially positioned at the top left corner (0,0) with length = 1 unit.

You are given a list of food's positions in row-column order. When a snake eats the food, its length and the game's score both increase by 1.

Each food appears one by one on the screen. For example, the second food will not appear until the first food was eaten by the snake.

When a food does appear on the screen, it is guaranteed that it will not appear on a block occupied by the snake.

Example:
Given width = 3, height = 2, and food = [[1,2],[0,1]].

Snake snake = new Snake(width, height, food);

Initially the snake appears at position (0,0) and the food at (1,2).

|S| | |
| | |F|

snake.move("R"); -> Returns 0

| |S| |
| | |F|

snake.move("D"); -> Returns 0

| | | |
| |S|F|

snake.move("R"); -> Returns 1 (Snake eats the first food and right after that, the second food appears at (0,1) )

| |F| |
| |S|S|

snake.move("U"); -> Returns 1

| |F|S|
| | |S|

snake.move("L"); -> Returns 2 (Snake eats the second food)

| |S|S|
| | |S|

snake.move("U"); -> Returns -1 (Game over because snake collides with border)
</pre>

### Solutions:

```java
/*
TC -> move = O(1)
SC -> O(max Snake Length)
*/

class SnakeGame {
private:
    int w;
    int h;
    int fIdx;
    vector<vector<int>> food;
    deque<vector<int>> posStack;
    set<vector<int>> posSet;
    enum Dir_t_ {
        R = 0,
        L,
        U,
        D
    } Dir_t;
    
    const vector<vector<int>> dirs = { {0, 1}, {0, -1}, {-1, 0}, {1, 0} };
    
    vector<int> getDirCoords(string dir) {
        if (dir == "R")
            return dirs[R];
        else if (dir == "L")
            return dirs[L];
        else if (dir == "U")
            return dirs[U];
        else if (dir == "D")
            return dirs[D];
        return {};
    }
    
public:
    SnakeGame(int width, int height, vector<vector<int>>& food) {
        this->w = width;
        this->h = height;
        this->fIdx = 0;
        this->food = food;
        posStack.push_front({0, 0});
        posSet.insert({0, 0});
    }
    
    // TC -> O(1)
    int move(string direction) {
        vector<int> dir = getDirCoords(direction);
        
        int r = posStack.front()[0] + dir[0];
        int c = posStack.front()[1] + dir[1];
        
        vector<int> headPos = {r, c};
        
        if (r < 0 || r >= h || c < 0 || c >= w) {
            // Snake ran out of bounds
            return -1;
        }
        
        if (fIdx < food.size() && food[fIdx][0] == r && food[fIdx][1] == c) {
            // Found the food, increment snake length
            fIdx++;
        } else {
            // We need to remove the last position from pStack as full snake would have gone through it
            vector<int> tailPos = posStack.back();
            posStack.pop_back();
            posSet.erase(tailPos);
        }
        
        if (posSet.find(headPos) != posSet.end()) {
            // Snake ran over itself
            return -1;
        }
                    
        posStack.push_front(headPos);
        posSet.insert(headPos);
        
        return posStack.size() - 1;
    }
};

```