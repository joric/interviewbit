# Evaluate Expression

https://www.interviewbit.com/problems/evaluate-expression


Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are +, -, *, /. Each operand may be an integer or another expression.

Examples:

  ["2", "1", "+", "3", "*"] -> ((2 + 1) * 3) -> 9
  ["4", "13", "5", "/", "+"] -> (4 + (13 / 5)) -> 6
  
## Solution

```cpp

// smallest

int Solution::evalRPN(vector<string> &str) {
    stack<int> s;
    for (string &x : str) {
        if (x=="+" || x=="-" || x=="*" || x == "/") {
            int b = s.top();
            s.pop();
            int a = s.top();
            s.pop();
            switch(x[0]) {
                case '+': s.push(a + b); break;
                case '-': s.push(a - b); break;
                case '*': s.push(a * b); break;
                case '/': s.push(a / b); break;
            }
        } else
            s.push(atoi(x.c_str()));
    }
    return s.top();
}



// fastest
int Solution::evalRPN(vector<string> &A) {
    stack<int> st;
    for (int i = 0; i < A.size(); i++) {
        if (A[i] == "+" || A[i] == "-" || A[i] == "*" || A[i] == "/") {
            int first = st.top();
            st.pop();
            int second = st.top();
            st.pop();
            if (A[i] == "+") {
                st.push(second + first);
            } else if (A[i] == "-") {
                st.push(second - first);
            } else if (A[i] == "*") {
                st.push(second * first);
            } else {
                st.push(second / first);
            }
        } else {
            st.push(atoi(A[i].c_str()));
        }
    }
    return st.top();
}


int Solution::evalRPN(vector<string> &A) {
    stack<int> s;
    unordered_map<string, int> opMapper;
    opMapper["+"] = 1;
    opMapper["-"] = 2;
    opMapper["*"] = 3;
    opMapper["/"] = 4;
    for (auto i = 0; i < A.size(); ++i) {
        //if(opMapper[A[i]]==1 || opMapper[A[i]]==2 || opMapper[A[i]]==3 || opMapper[A[i]]==4)
        if (A[i] == "+" || A[i] == "-" || A[i] == "*" || A[i] == "/") {
            if (!s.empty()) {
                int b = s.top();
                s.pop();
                int a = s.top();
                s.pop();
                switch (opMapper[A[i]]) //Clever way: A[i][0] returns a char since [i][0] gives first char.
                {                       //Much better than using a mao.
                case 1:
                    s.push(a + b);
                    break;
                case 2:
                    s.push(a - b);
                    break;
                case 3:
                    s.push(a * b);
                    break;
                case 4:
                    if (b == 0)
                        return 0;
                    s.push(a / b);
                    break;
                default:
                    break;
                }
            }
        } else
            //s.push(atoi(A[i].c_str())); //string to char to int method.
            s.push(stoi(A[i], nullptr, 10));
    }

    return s.top();
}
```