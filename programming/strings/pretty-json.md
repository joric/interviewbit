# Pretty Json

https://www.interviewbit.com/problems/pretty-json


This is more of a parsing/simulation problem. Think about the corner cases wisely.


This is more of a parsing problem.

Make sure you take a lot of time thinking about the corner cases and structure of the code before you start coding.

Fixing corner cases on the fly can make your code really ugly.

Note the following:

1) '{', '[' have the same effect on the printing

2) '}', ']' have the same effect as well

3) ':' and ',' are the only other 2 characters that matter.

Think about the behavior when you encounter the following characters.

Also think about the behavior based on the following character.


## Solution

```cpp

vector<string> Solution::prettyJSON(string A) 
{
    int n = A.size(),i,j;
    vector< string >ans;
    int tbctr = 0;
    i = 0;
    bool flag = false;
    while(i<n)
    {
        string row = "";
        for(j=0;j<tbctr;j++)
            row += '\t';
            flag = false;
        for(j=i;j<n;j++)
        {
            if(A[j] == '[' || A[j] == '{')
            {
                if(flag)
                    ans.push_back(row);
                row = "";
                for(int k=0;k<tbctr;k++)
                    row += '\t';
                row += A[j];
                ans.push_back(row);
                i = j+1;
                tbctr++;
                
                break;
            }
            if(A[j] == ']' || A[j] == '}')
            {
                tbctr--;
                if(flag)
                    ans.push_back(row);
                row = "";
                for(int k=0;k<tbctr;k++)
                    row += '\t';
                row += A[j];
                if(j + 1 < n && A[j+1] == ',')
                    row += A[++j];
                ans.push_back(row);
                i = j+1;
                break;
            }
            row += A[j];
            flag = true;
            i = j+1;
            if(A[j] == ',')
            {
                if(flag)
                    ans.push_back(row);
                i = j+1;
                break;
            }
        }
      //  i = j+1;
    //    i++;
    }
    return ans;
}
```