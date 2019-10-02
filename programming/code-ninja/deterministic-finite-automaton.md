# Deterministic Finite Automaton

https://www.interviewbit.com/problems/deterministic-finite-automaton/

Deterministic finite automaton(DFA) is a finite state machine that accepts/rejects finite strings of symbols and only produces a unique computation (or run) of the automation for each input string.

DFAs can be represented using state diagrams. For example, in the automaton shown below, there are three states: S0, S1, and S2 (denoted graphically by circles). The automaton takes a finite sequence of 0s and 1s as input. For each state, there is a transition arrow leading out to a next state for both 0 and 1. Upon reading a symbol, a DFA jumps deterministically from a state to another by following the transition arrow. For example, if the automaton is currently in state S0 and current input symbol is 1 then it deterministically jumps to state S1. A DFA has a start state (denoted graphically by an arrow coming in from nowhere) where computations begin, and a set of accept states (denoted graphically by a double circle) which help define when a computation is successful.

![](https://upload.wikimedia.org/wikipedia/commons/9/94/DFA_example_multiplies_of_3.svg)

These are some strings above DFA accepts,
```
0
00
000
11
110
1001
```
You are given a DFA in input and an integer N. You have to tell how many distinct strings of length N the given DFA accepts. Return answer modulo 109+7.

### Notes


Assume each state has two outgoing edges(one for 0 and one for 1). Both outgoing edges won't go to the same state.
There could be multiple accept states, but only one start state.
A start state could also be an accept state.
Input format

* States are numbered from 0 to K-1, where K is total number of states in DFA.
* You are given three arrays A, B, C and two integers D and N.
* Array A denotes a 0 edge from state numbered i to state A[i], for all 0 <= i <= K-1
* Array B denotes a 1 edge from state numbered i to state B[i], for all 0 <= i <= K-1
* Array C contains indices of all accept states.
* Integer D denotes the start state.
* Integer N denotes you have to count how many distinct strings of length N the given DFA accepts.

### Constraints

1 <= K <= 50

1 <= N <= 104

### Example
```
For the DFA shown in image, input is
A = [0, 2, 1]
B = [1, 0, 2]
C = [0]
D = 0

Input 1
-
N = 2
Strings '00' and '11' are only strings on length 2 which are accepted. So, answer is 2.

Input 2
-
N = 1
String '0' is the only string. Answer is 1.
```

## Solution
```python
class Solution:
    def automata(self, A, B, C, D, E):
        K = len(A)
        prev = [0] * K
        prev[D] = 1
        for i in range(1, E + 1):
            tmp = [0] * K
            for j in range(K):
                if not prev[j]:
                    continue
                tmp[A[j]] += prev[j]
                tmp[B[j]] += prev[j]
            prev = tmp
        answer = 0
        for i in C:
            answer += prev[i]
        return answer % (10 ** 9 + 7)
```
