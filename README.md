# striver-sde-sheet
#day 1  08/08/2023
Q1 73. Set Matrix Zeroes
sol:
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        // using inplace algorithm
        int col0=1;
        int row = matrix.size();
        int col = matrix[0].size();

        // step1 traverse the matrix and mark 0 in m[i][0] & m[0][j] 
        for(int i=0 ; i<row ; i++){
            for(int j =0 ; j<col ; j++){
                if(matrix[i][j]==0){

                    //mark ith row
                    matrix[i][0]=0;
                    
                    //mark jth row
                    if(j!=0)
                    matrix[0][j]=0;
                    else
                    col0=0;


                }
            }
        }
        //step 2 mark with 0 (1,1) to (row-1,col-1)
        for(int i=1;i<row;i++){
            for(int j=1;j<col;j++){
                if(matrix[i][0]==0 ||matrix[0][j]==0){
                    matrix[i][j]=0;
                }
            }
        }
        //step 3 now mark 1st column and 1st row
        if(matrix[0][0]==0){
            for(int j=0;j<col ; j++){
                matrix[0][j]=0;
            }
        }
         if(col0==0){
            for(int i=0;i<row ; i++){
                matrix[i][0]=0;
            }
        }
    }
};
--------------------------------------------------------------------------------------------------------------------------------------------------------------------

#day2  09/08/2023
Q2 118. Pascal's Triangle
Given an integer numRows, return the first numRows of Pascal's triangle.

In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:

sol: 
class Solution {
public:
int ncr(int n , int r){
            long int res =1;
            //calculation of ncr 
            for(int i=0;i<r;i++){
                res = res*(n-i);
                res = res/(i+1);
            }
            return res;
        }

    vector<vector<int>> generate(int numRows) {
        
        

        //calculation for whole pascal triangle
        vector<vector<int>>ans;
        for(int row = 1; row <= numRows;row++){
            vector<int>temp;
            for(int col =1 ; col<= row;col++){
                temp.push_back(ncr(row-1 , col-1));
            }
            ans.push_back(temp);
        }
        return ans;
    }
};



Q3 31. Next Permutation
Problem Statement: Given an array Arr[] of integers, rearrange the numbers of the given array into the lexicographically next greater permutation of numbers.

If such an arrangement is not possible, it must rearrange to the lowest possible order (i.e., sorted in ascending order).

sol:

class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int ind = -1;
        int n= nums.size();
        //finding breakpoint 
        for(int i=n-2;i>=0;i--){
            if(nums[i]<nums[i+1]){
            ind= i;
            break;
            }
        }
        //if breakpoint doesnot exist
        if(ind == -1){
            reverse(nums.begin(), nums.end());
            return;
        }
        //finding next greater element >nums[i] from i+1 to n-1
        //and swap it
        for(int i=n-1; i>=0;i--){
            if(nums[i]>nums[ind]){
                swap(nums[i], nums[ind]);
                break;
            }
        }
        //now reverse in the right half 
        reverse(nums.begin()+ind+1 , nums.end());
        return ;
    }
};

------------------------------------------------------------------------------------------------------------------------------------------------------------------
#day3 10/08/2023
Q4 53. Maximum Subarray
Kadane’s Algorithm : Maximum Subarray Sum in an Array
Problem Statement: Given an integer array arr, find the contiguous subarray (containing at least one number) which
has the largest sum and returns its sum and prints the subarray.

sol: 
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int count = INT_MIN;
        int sum = 0;
        int n= nums.size();
        for(int i=0;i<n;i++){
            sum += nums[i];
            if(sum>count)
            count =sum;

            if(sum<0)
            sum =0;
        }
        return count;
        
    }
};


Q5 75. Sort Colors
Sort an array of 0s, 1s and 2s
Problem Statement: Given an array consisting of only 0s, 1s, and 2s. Write a program to in-place sort the array without using inbuilt sort functions.
( Expected: Single pass-O(N) and constant space)

sol:
class Solution {
public:
    void sortColors(vector<int>& nums) {
        //using DNF algorithm
        int n = nums.size();
        int low = 0 , mid = 0; int high = n-1;
        while(mid<=high){
            if(nums[mid]==0){
                swap(nums[low] , nums[mid]);
                low++ ;
                mid++;
            }
            else if(nums[mid]==1){
                mid++;
            }else{
                swap(nums[mid], nums[high]);
                high--;
            }
            
        }

        }
    
};


Q6 121. Best Time to Buy and Sell Stock
Problem Statement: You are given an array of prices where prices[i] is the price of a given stock on an ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock. Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

sol:
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n= prices.size();
        int maxPro = 0;
       int  minPrice = INT_MAX;
        for(int i=0; i<n ; i++){
          minPrice = min(minPrice, prices[i]);
          maxPro = max(maxPro , prices[i]- minPrice);
        }

return maxPro;

    }
};

------------------------------------------------------------------------------------------------------------------------------------------------------------------
#day4
Q7
48. Rotate Image
Problem Statement: Given a matrix, your task is to rotate the matrix 90 degrees clockwise.

sol:
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();

        // transposing the matrix
        for(int i=0 ; i<n ; i++){
            for(int j=0;j<i ; j++){
                swap(matrix[i][j] , matrix[j][i]);
            }
        }
        // reversing each row
        for(int i =0;i<n;i++){
            reverse(matrix[i].begin() , matrix[i].end());
        }
    }
};
------------------------------------------------------------------------------------------------------------------------------------------------------------------
#day5
Q8 56. Merge Intervals
Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.
sol:
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
         int  n = intervals.size();
        //sorting array intervals
        sort(intervals.begin() , intervals.end());

        //creating vector variable 'ans' to store ans
        vector<vector<int>>ans;
        for(int i = 0 ; i< n ; i++){
            if(ans.empty() || intervals[i][0]> ans.back()[1]){
                ans.push_back(intervals[i]);
            }
            else{
                ans.back()[1] = max(ans.back()[1] , intervals[i][1]);
            }
        }
        return ans;
    }
};
------------------------------------------------------------------------------------------------------------------------------------------------------------------
Q9 88. Merge Sorted Array
Problem statement: Given two sorted arrays arr1[] and arr2[] of sizes n and m in non-decreasing order. Merge them in sorted order. Modify arr1 so that it contains the first N elements and modify arr2 so that it contains the last M elements.
sol:
class Solution {
public:
void swapIfGreater(vector<int>& nums1, vector<int>& nums2, int ind1, int ind2) {
    if (nums1[ind1] > nums2[ind2]) {
        swap(nums1[ind1], nums2[ind2]);
    }
}
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        //length of single sorted array
        int len = m+n;
        //initial gap 
        int gap = len/2 + len%2;
         while(gap>0){
             int left =0;
             int right = left + gap;
             while(right <len){
                 if(left<m && right >=m){
                     swapIfGreater(nums1 , nums2 ,left , right - m);
                 }else if(left>=m){
                     swapIfGreater(nums2 , nums2 ,left-m , right - m);
                 }else{
                     swapIfGreater(nums1 , nums1 ,left , right);
                 }
                 left++ ; right++;
             }
             //if gap is 1
             if(gap == 1)
             break;
             //otherwise calculate new gap
             gap = gap/2 + gap%2;
         }
    }
};
