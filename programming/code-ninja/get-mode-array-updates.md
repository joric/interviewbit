# Get Mode Array Updates

https://www.interviewbit.com/problems/get-mode-array-updates/

You are given an array of N positive integers, A1, A2 ,…, AN.
Also, given a Q updates of form:

1. `[i,j]: Update Ai = j. 1 <= i <= N`

Perform all updates and after each such update report mode of the array. Therefore, return an array of Q elements, where ith element is mode of the array after ith update has been executed.

### Notes

Mode is the most frequently occuring element on the array.
If multiple modes are possible, return the smallest one.
Update array input is via a Q*2 array, where each row represents a update.

### For example

```
A=[2, 2, 2, 3, 3]

Updates=    [ [1, 3] ]
            [ [5, 4] ]
            [ [2, 4] ]

A = [3, 2, 2, 3, 3] after 1st update.
3 is mode.

A = [3, 2, 2, 3, 4] after 2nd update.
2 and 3 both are mode. Return smaller i.e. 2.

A = [3, 4, 2, 3, 4] after 3rd update.
3 and 4 both are mode. Return smaller i.e. 3.

Return array [3, 2, 3].
```
### Constraints
```
1 <= N, Q <= 105
1 <= j, Ai <= 109
```

## Solution
```java
public class Solution {
    HashMap<Integer, Integer> map;
    TreeMap<Integer, TreeMap<Integer, Integer>> map2;
    public ArrayList<Integer> getMode(ArrayList<Integer> A, ArrayList<ArrayList<Integer>> B) {
        map = new HashMap<Integer, Integer>();
        map2 = new TreeMap<>();
        ArrayList<Integer> result = new ArrayList<Integer>();

        for(int i : A) {
            if(map.containsKey(i))
                map.put(i, map.get(i) +1);
            else
                map.put(i, 1);
        }
        for (Map.Entry m : map.entrySet()) {
            int freq = (int)m.getValue();
            TreeMap<Integer, Integer> x = new TreeMap<>();
            if (map2.containsKey(freq) == true) {
                x = map2.get(freq);
            }
            x.put((int)m.getKey(),1);
            map2.put(freq, x);
        }
        for(ArrayList<Integer> update : B){
            int index = update.get(0);
            int num = update.get(1);

            int toUpdate = A.get(index - 1);

            if(map.get(toUpdate) != null) {
                int freq = map.get(toUpdate);
                TreeMap<Integer, Integer> temp = map2.get(freq);
                if (temp.size() == 1) {
                    map2.remove(freq);
                }
                else {
                    temp.remove(toUpdate);
                    map2.put(freq, temp);
                }
                if (freq == 1) {
                    map.remove(toUpdate);
                }
                else {
                    map.put(toUpdate, freq - 1);
                    int z = freq-1;
                    TreeMap<Integer, Integer> tt = new TreeMap<>();
                    if (map2.containsKey(z)) {
                        tt = map2.get(z);
                    }
                    tt.put(toUpdate, 1);
                    map2.put(z, tt);
                }
            }
            A.set(index - 1, num);
            if(map.containsKey(num)) {
                int tt = map.get(num);
                TreeMap<Integer, Integer> w = map2.get(tt);
                if (w.size() == 1) {
                    map2.remove(tt);
                }
                else {
                    w.remove(num);
                    map2.put(tt, w);
                }
                map.put(num, tt+1);
                TreeMap<Integer, Integer> q = new TreeMap<>();
                if (map2.containsKey(tt+1)) {
                    q = map2.get(tt+1);
                }
                q.put(num,1);
                map2.put(tt+1, q);
            }
            else {
                map.put(num, 1);
                TreeMap<Integer, Integer> qq = new TreeMap<>();
                if (map2.containsKey(1)) {
                    qq = map2.get(1);
                }
                qq.put(num, 1);
                map2.put(1, qq);
            }
            int rr = (int)map2.lastKey();
             TreeMap<Integer, Integer> xyz = map2.get(rr);
            result.add((int)(xyz.firstKey()));
        }
        return result;
    }
}
```
