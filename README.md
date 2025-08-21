# NeetCode-150-Question-on-Leetcode
NeetCode 150 Question on Leetcode  

Aapko ek array diya gaya hai, aur usme se k sabse zyada frequent elements nikalne hain. Matlab, jo numbers sabse zyada baar aaye hain, unme se top k chahiye.
Example:
347. Top K Frequent Elements
nums = [1,1,1,2,2,3], k = 2
freq:
1 → 3 times
2 → 2 times
3 → 1 time

Top 2 = [1, 2]

🔹 Approach
Frequency count banayenge (hashmap/dictionary se).
Heap (priority queue) ya bucket sort ka use karenge top k elements nikalne ke liye.

Step 1 — Frequency count (unordered_map<int,int> freq)

Step 2 — Build min-heap of size ≤ k We iterate over freq entries. (Iteration order of unordered_map can vary; yahan clear samajhne ke liye order liya: num = 1, 2, 3.) 
Heap stores pairs (count, num) and top = smallest count

Step 3 — Extract results We pop from heap and append second (num) to result:
Pop top (2,2) → result = [2]
Pop top (3,1) → result = [2, 1] 
Function returns result = [2, 1].

Summary (quick)
freq map = {1:3, 2:2, 3:1}
After maintaining min-heap of size k=2, heap kept {(2,2),(3,1)}
Extract → result [2,1] (or reverse to [1,2] for descending frequency)

Time complexity ko O(n log k) ya O(n) ke andar rakhna hoga.
Space Complexity = O(n)

class Solution {
public:

    vector<int> topKFrequent(vector<int>& nums, int k) {
        // Step 1: frequency count
        unordered_map<int, int> freq;
        for(int num : nums){
            freq[num]++;
        }
        
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> minheap;
        
        // Step 2: min heap (freq ke basis pe)
        for(auto& [num, count] : freq){
            minheap.push({count, num});
            if(minheap.size() > k){
                minheap.pop();
            }   
        }
        // Step 3: extract top k elements
        vector<int>ans;
        while(!minheap.empty()){
            ans.push_back(minheap.top().second);
            minheap.pop();
        }
        return ans;
    }
};
