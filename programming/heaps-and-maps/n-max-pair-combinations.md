# N Max Pair Combinations

https://www.interviewbit.com/problems/n-max-pair-combinations


Brute force is to find all combinations pair sum O(N Square) and return top N max elements.
Can Sorting Work better?


Sort both the arrays in ascending order.
Let us take priority queue (heap).
First max element is going to be the sum of the last two elements of array A and B i.e. (A[n-1] + B[n-1]).
Insert that in heap with indices of both array i.e (n-1, n-1).
Start popping from heap (n-iterations).
And insert the sum (A[L-1]+A[R]) and (A[L]+B[R-1]).
Take care that repeating indices should not be there in the heap (use map for that).

## Solution

```cpp

// editorial

vector<int> Solution::solve(vector<int> &A, vector<int> &B) {
    priority_queue<pair<int, pair<int, int> > > hp;
    set<pair<int, int> > S;
    int n = A.size();
    sort(A.begin(), A.end());
    sort(B.begin(), B.end());

    hp.push(make_pair(A[n-1]+B[n-1], make_pair(n-1, n-1)));
    S.insert(make_pair(n-1, n-1));

    vector<int> ans;
    int k=n;
    while(k--){
        pair<int, pair<int, int> > top = hp.top();
        hp.pop();
        ans.push_back(top.first);
        int L = top.second.first;
        int R = top.second.second;
        
        if( R>0 && L>=0  && S.find(make_pair(L,R-1)) == S.end() ){
            hp.push(make_pair(A[L]+B[R-1], make_pair(L,R-1)));
            S.insert(make_pair(L,R-1));
        }
        if( R>=0  && L>0 && S.find(make_pair(L-1, R))==S.end()){
            hp.push(make_pair(A[L-1]+B[R], make_pair(L-1,R)));
            S.insert(make_pair(L-1, R));
        }
    }
    return ans;
}
/*

vector<int> Solution::solve(vector<int> &A, vector<int> &B) {

    int i = 1, j = 1;
    int s = A.size();
    int cnt = s;
    int eq = 0;

    sort(A.begin(), A.end());
    sort(B.begin(), B.end());

    vector<int> ans;
    set<pair<int, int>> st;
    priority_queue<pair<int, pair<int, int>>> pq;

    pq.push(make_pair(A[s - 1] + B[s - 1], make_pair(s - 1, s - 1)));
    st.insert(make_pair(s - 1, s - 1));

    while (cnt) {

        pair<int, pair<int, int>> tuple;

        tuple = pq.top();
        pq.pop();
        ans.push_back(tuple.first);

        int ino1 = tuple.second.first;
        int ino2 = tuple.second.second;

        if (st.find(make_pair(ino1, ino2 - 1)) == st.end()) {

            pq.push(make_pair(A[ino1] + B[ino2 - 1], make_pair(ino1, ino2 - 1)));
            st.insert(make_pair(ino1, ino2 - 1));
        }

        if (st.find(make_pair(ino1 - 1, ino2)) == st.end()) {

            pq.push(make_pair(A[ino1 - 1] + B[ino2], make_pair(ino1 - 1, ino2)));
            st.insert(make_pair(ino1 - 1, ino2));
        }

        --cnt;
    }

    return ans;
}
*/
```