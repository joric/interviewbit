# Byes

https://www.interviewbit.com/problems/byes/

Recently there was a single-elimination tournament. The tournament consisted of multiple rounds.
In each round the players were divided into pairs, each pair played a game, and the loser of each game was
eliminated from the tournament. Once there was only a single player left, the tournament terminated and that
player was declared its winner.

If the number of players in a round is odd, it is impossible to divide all of them into pairs. Whenever such
a situation arised, one of the players was awarded a bye and advanced into the next round without playing a game.

Given two integers A and B. You know that the number of players in the tournament was at least A,
and that the total number of byes awarded to players during the tournament was B.

Find and return the smallest possible number of players.

### Input Format

The first argument given is integer A.

The second argument given is integer B.

### Output Format

Return the smallest possible number of players.

### Constraints

1 <= A <= 2^28

0 <= B <= 28

### For Example
```
Input 1:
    A = 3
    B = 1
Output 1:
    3
Explanation 1:
    A tournament with three people looks as follows:
    1. In the first round one person gets a bye and the remaining two people play a game. Two people advance from this round.
    2. In the second round the two remaining people face off. The loser is out, the winner wins the tournament.
    As the tournament with three people had exactly one bye, this is what we are looking for.

Input 2:
    A = 4
    B = 1
Output 2:
    6
Explanation 2:
    In a tournament with four players there are no byes. 
    In a tournament with five players there are two byes (one in the first round and one in the second). 
    In a tournament with six players there is a single bye (in the second round), hence six is the correct answer for this test case.
```

## Solution
### Editorial
```cpp
int numZeroes(long long v) {
    if (v == 0) return 0;
    return 64 - __builtin_clzll(v) - __builtin_popcountll(v);
  }

int Solution::solve(int lowerBound, int numberOfByes) {
    if (numberOfByes > 0) {
      lowerBound = max(lowerBound, 2);
    }
    lowerBound --;
    while (numZeroes(lowerBound) < numberOfByes) {
      lowerBound += (lowerBound & (-lowerBound));
    }
    for (long long v = 1; numZeroes(lowerBound) > numberOfByes; v <<= 1) {
      lowerBound |= v;
    }
    return lowerBound+1;
}
```
