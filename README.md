Game-AIs
========

Bots and AIs of varying complexity 

TicTacToe, DotsAndBoxes and AntiChess were originally made for a competition held at https://www.hackerrank.com (I placed third). The others were done just for fun.

**Sudoku, NQueens - DancingLinks**
I implemented the very interesting Dancing Links (http://en.wikipedia.org/wiki/Dancing_Links) algorithm and solved a couple problems using it.
Dancing Links finds all the solutions to the exact cover problem and, it turns out, quite a few well known problems can be reformulated in terms of the exact cover problem.

Thus all the actual solutions do is cast the problem as a matrix representing the relevant constraints and then call dancing links on the matrix. Specifically I implemented:
* Sudoku (http://en.wikipedia.org/wiki/Sudoku). Encodes the three intuitive restrictions forcing each number to appear on every row, column and in every box. It also requires an extra restriction saying that only one number can appear in each cell. (see: http://en.wikipedia.org/wiki/Exact_cover#Sudoku for details). It can solve blank or already started boards quite efficiently. A standard sudoku board is solved instantly and a 25x25 board takes under 20 seconds.
* N Queens (http://en.wikipedia.org/wiki/N-queens). The four intuitive restrictions are all that are necessary here (one queen on every row, column, principal diagonal and secondary diagonal). This implementation is quite efficient for a non tailor-made method, with a 17x17 board being solved in under 10 seconds.

**Boggle** - http://en.wikipedia.org/wiki/Boggle
Finds all legal words included in the dictionary file it is given. It is quite fast, being able to solve a boggle board 100 times bigger than the standard one in under 5 minutes.

First a I implement a trie(http://en.wikipedia.org/wiki/Trie) that is used to store the dictionary.
I solve the problem by looking at prefixes comprised of an increasing number of letters and, at every step, discarding those that can no longer form a word.
I start with prefixes of length 1, as dictated by the individual letters present on the board. For each of these I look at all possible neighbours and if the new prefix of length 2 is still valid it is kept. The algorithm is then repeated untill I run out of valid prefixes. At every step I also check if a complete word has been formed, in which case the word is saved.

The important points of this algorithm are that:
* The number of neighbours I have to visit is also limited. 8 in the standard Boggle rules
* The solutions themselves are dictionary words, therefore this algorithm will terminate after a short number of iterations. In my dictionary the longest words have length less than 15.
* The previous two facts in conjunction with the elimination of all non-prefix strings means that at every step we are only looking at a number of strings within a constant factor of the number of final solutions.
* The trie makes it possible for us to check in constant time whether a new prefix is now a complete word and whether it is still a valid prefix.

This means the runtime is O(number of solutions)

**DotsAndBoxes** - http://en.wikipedia.org/wiki/Dots_and_Boxes
This game is actually surprisingly complex to solve. I have implemented a number of bots, each incrementally improving on the last as well as an engine that can run them repeteadly against one another or against a human player. 
In short:
* Bot0 - makes random choices
* Bot1 - greedy decisions, tries to make a capture, if it cannot it tries to avoid ceding a capture on the next turn
* Bot2 - still greedy, but is more aware of the structure of the game, namely that some captures lead to a long chain of subsequent captures (chains) and that some loses can end up benefiting you, if they force your opponent into giving up a chain.
* Bot3 - tries to calculate exactly when a strategic loss should be given and taken. This involves an analysis of the whole board and an estimation of future opponent bahaviour
* Bot4 - this time explicit simulation of future turns is done, via minmax with alpha-beta prunning
* Bot5 - same as Bot4 except it also has a couple heuristics to use when everything else fails

The main ideas for the DotsAndBoxes bots were taken from this paper: http://www.aaai.org/ocs/index.php/AAAI/AAAI12/paper/view/5126

**AntiChess** - http://en.wikipedia.org/wiki/Antichess
As for DotsAndBoxes, this game also has a number of bots and an engine to run them against each other or against a human.
In short:
* Bot0 - makes random choices
* Bot1 - tries to make a move that will force the opponent to capture, but won't force me to recapture the next turn
* Bot2 - if above strategy fails, tries to move to a place that is safe for the next turn (i.e. this is equivalent to a 2 move deep search of the game tree)
* Bot3 - basic minmax implementation, manages to go 1 or 2 moves deeper than the previous bot
* Bot4 - smarter minmax search, now with alpha-beta pruning. Goes a couple levels deeper still.

**TicTacToe** - http://en.wikipedia.org/wiki/Tic-tac-toe 
Basic minmax implementation that never loses. Additionally tries to exploit weak players by calculating the probabilistic value of moves that have the same minmax given value(this is useful because minmax assumes the opponent plays optimally and therefore almost all moves are predicted as leading to a tie). This probabilistic value is used to differentiate between these moves, giving us some hope of profiting from an opponent mistake.
