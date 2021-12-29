# 321 Create Maximum Number

### Problem:

Given two arrays of length m and n with digits 0-9 representing two numbers. Create the maximum number of length k &lt;= m + n from digits of the two. The relative order of the digits from the same array must be preserved. Return an array of the k digits. You should try to optimize your time and space complexity.

Example 1:  
nums1 = \[3, 4, 6, 5\]  
nums2 = \[9, 1, 2, 5, 8, 3\]  
k = 5  
return \[9, 8, 6, 5, 3\]

Example 2:  
nums1 = \[6, 7\]  
nums2 = \[6, 0, 4\]  
k = 5  
return \[6, 7, 6, 0, 4\]

Example 3:  
nums1 = \[3, 9\]  
nums2 = \[8, 9\]  
k = 3  
return \[9, 8, 9\]

### Solutions:

```java
/*
Idea -

K elements can be picked as i elements from nums1 and k-i elements from nums2.
We slice two lists nums1 and nums2 by choosing series of values for i
We create biggest number possible from each list for given value of i or k-i
We combine the two lists to get the biggest number for given slice.
We choose maximum number for all slices.
Code below -
*/
class Solution {

private:
    //Compare function for two generic collections.
    //if collection contains non primitive types then it must overload
    //operator '<'
    
    //returns true if second list is greater in value than first one
    //if all elements are equal then it returns true if second list is
    //larger in size
    
    template <class InputIterator1, class InputIterator2>
      bool lexicographical_compare (InputIterator1 first1, InputIterator1 last1,
                                    InputIterator2 first2, InputIterator2 last2)
    {
      while (first1!=last1)
      {
        if (first2==last2 || *first2<*first1) return false;
        else if (*first1<*first2) return true;
        ++first1; ++first2;
      }
      return (first2!=last2);
    }
public:
    
    //subproblem 1 = create the greatest number with K digits from a single list
    vector<int> maxNumber(vector<int> nums, int k) {
        int drop = nums.size()-k; //number of digits to be dropped
        vector<int> out;
        for(int num : nums) {
            //if there are digits left to be dropped and if we find
            //a digit that is greater than the digit we have in our 
            //output array we drop the digits and add that digit
            while(drop > 0 && out.size() != 0 && out.back() < num) {
                drop--;
                out.pop_back();
            }
            out.push_back(num);
        }
        out.resize(k); // this is necessary if there are more than K digits in out
        return out;
    }
    
    /*
      subproblem 2 = create largest number from the two lists, containing
      digits [0-9]. The size of output number is sum of size of two
      lists
    */
    vector<int> maxNumber(vector<int> nums1, vector<int> nums2) {
        auto start1 = nums1.begin(), end1 = nums1.end(),
             start2 = nums2.begin(), end2 = nums2.end();
        vector<int> out;
        while(start1 != end1 || start2 != end2) {
            if(lexicographical_compare(start1,end1,start2,end2)) {
                out.push_back(*start2++);
            }
            else {
                out.push_back(*start1++);
            }
        }
        return out;
    }
    
    vector<int> maxNumber(vector<int>& nums1, vector<int>& nums2, int k) {
        int n1 = nums1.size(), n2 = nums2.size();
        vector<int> best;
        /* 
           The start and end range of for the "for loop" is computed based on
           number of min elements we need to pick from nums1 and number of maximum
           elements we can pick from nums1. if k > n2 then we must at least pick up
           k-n2 elements from n1
           
           
           Example 1
           consider n1 = 4 and n2 = 6 and k = 5
           Result can be constructed in following ways 
           (i:0, k-i:5), (i:1, k-i:4), (i:2, k-i:3), (i:3, k-i:2), (i:4, k-i:1)
           where i is number of elements picked from nums1 and k-i is number of elements
           picked from nums2. Please note
           --> (i:5, k-i:0) is not valid since n1=4
           
           Example 2
           Now consider n1 = 6 and n2 = 4 and k = 5
           (i:1, k-i:4), (i:2, k-i:3), (i:3, k-i:2), (i:4, k-i:1), (i:5, k-i:0)
           
           hence starting index for i that is associated with nums1 is 
           max(0, k-n2) (in first example k-n2 = -1, in second example k-n2 = 1)
           
           end index for i associated with nums1 is 
           min(k,n1) (in first example i can only go up to 4 since n1 = 4, in second example
                      i can only go up to 5 since k = 5)
                      
          Example 3
          n1 = 3, n2 = 4, k = 5
          we must pick at least 1 element from nums1 (k-n2 = 1) also i can only go up to 3 since
          n1 is capped at 3
          
          Example 4
          
          n1 = 5, n2=3, k = 4
          we must pick up at least 1 element from nums1 (k-n2 = 1) and i can go up to 4
           
        */
          for(int i = max(0,k-n2); i <= min(k,n1); i++) {
            
            if(best.size() == 0) {
                best = maxNumber(maxNumber(nums1,i),
                                 maxNumber(nums2,k-i));
            }
            else {
                vector<int> temp = maxNumber(maxNumber(nums1,i),
                                             maxNumber(nums2,k-i));
                if(lexicographical_compare(best.begin(), best.end(),
                                           temp.begin(),temp.end())) {
                    best = temp;
                }
            }
            //vector class overloads "<" operator hence you could simply use the 
            //statement below, above code is only to make things simple.
            //best = max(best,maxNumber(maxNumber(nums1,i),maxNumber(nums2,k-i)));
        }
        return best;
    }    
};
```



