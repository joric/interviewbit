# Hotel Bookings Possible

https://www.interviewbit.com/problems/hotel-bookings-possible/

```cpp
bool Solution::hotel(vector<int> &arrive, vector<int> &depart, int K) {

    sort(arrive.begin(), arrive.end());
    sort(depart.begin(), depart.end());

    for (int i=0, j=0, c=0; i<arrive.size() && j<depart.size();) {
        if (arrive[i]<depart[j])
            c++, i++;
        else
            c--, j++;
        if(c>K)
            return false;
    }

    return true;
}

/*

// The idea is store arrival and departure times in an auxiliary array with an additional marker
// to indicate whether the time is arrival or departure. Now sort the array. Process the sorted
// array, for every arrival increment active bookings. And for every departure, decrement.
// Keep track of maximum active bookings. If the count of active bookings at any moment
// is more than k, then return false. Else return true.

bool Solution::hotel(vector<int> &arrive, vector<int> &depart, int K) {
    vector<pair<int, int> > ans; 
    
    int n = arrive.size();
  
    // create a common vector both arrivals 
    // and departures. 
    for (int i = 0; i < n; i++) { 
        ans.push_back(make_pair(arrive[i], 1)); 
        ans.push_back(make_pair(depart[i], 0)); 
    } 
  
    // sort the vector 
    sort(ans.begin(), ans.end()); 
  
    int curr_active = 0, max_active = 0; 
  
    for (int i = 0; i < ans.size(); i++) { 
  
        // if new arrival, increment current 
        // guests count and update max active  
        // guests so far 
        if (ans[i].second == 1) { 
            curr_active++; 
            max_active = max(max_active,  
                            curr_active); 
        } 
  
        // if a guest departs, decrement  
        // current guests count. 
        else
            curr_active--; 
    } 
  
    // if max active guests at any instant  
    // were more than the available rooms,  
    // return false. Else return true. 
    return (K >= max_active); 
}
*/
```

