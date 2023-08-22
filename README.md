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
#day6 Q9 88. Merge Sorted Array
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

------------------------------------------------------------------------------------------------------------------------------------------------------------------
#day7 Q10 287. Find the Duplicate Number
Problem Statement: Given an array of N + 1 size, where each element is between 1 and N. Assuming there is only one duplicate number, your task is to find the duplicate number.
sol:
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int slow =nums[0];
        int fast =nums[0];
        
        do{
            slow =nums[slow];
            fast = nums[nums[fast]];
        }while(slow!=fast);

        fast =nums[0];
        while(slow!=fast){
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
};
------------------------------------------------------------------------------------------------------------------------------------------------------------------
#day8 Q11 Missing and repeating numbers
Problem Statement: You are given a read-only array of N integers with values also in the range [1, N] both inclusive. Each integer appears exactly once except A which appears twice and B which is missing. The task is to find the repeating and missing numbers A and B where A repeats twice and B is missing.
sol:
#include <bits/stdc++.h>

pair<int,int> missingAndRepeating(vector<int> &arr, int n)
{
	// Write your code here 
	//sum of first n numbers
	long long Sn = (n*(n+1))/2;
	//sum of square of first n numbers
	long long S2n = (n*(n+1)*(2*n+1))/6; 
    
	long long S = 0;
	long long S2 = 0;
	for(int i=0;i<n;i++){
		// sum of all niumbers in a given array
		S += arr[i];
		// sum of square of all numbers in a given array
		S2 += (long long)arr[i] * (long long)arr[i];		
	}
	
	//here X is repeating no. and Y is a missing no.
	
	// S-Sn = X-Y
	long long val1 = S-Sn ;
	
	// S2-S2n = X^2 - Y^2
	long long val2 = S2 - S2n;
	
	//find value X+Y = (S2 -S2n)/(X-Y) 
	val2 = val2 / val1;
	
	// Here, X-Y = val1 and X+Y = val2:
    long long x = (val1 + val2) / 2;
    long long y = x - val1;
	
	return {(int)y,(int)x};
	
}


Q12 Count Inversions
Problem Statement: Given an array of N integers, count the inversion of the array (using merge-sort).

What is an inversion of an array? Definition: for all i & j < size of array, if i < j then you have to find pair (A[i],A[j]) such that A[j] < A[i].
sol:
#include <bits/stdc++.h> 
long long getInversions(long long *arr, int n){
    // Write your code here.
    int count = 0;
    for(int i=0;i<n;i++){
        
        for(int j=i+1 ;j<n;j++){
            if(arr[j]<arr[i]){
                count++;
            }
        }
    }
    return count;
}


Q13 74. Search a 2D Matrix
You are given an m x n integer matrix matrix with the following two properties:

Each row is sorted in non-decreasing order.
The first integer of each row is greater than the last integer of the previous row.
Given an integer target, return true if target is in matrix or false otherwise.

You must write a solution in O(log(m * n)) time complexity.

sol:
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int  n = matrix.size();
        int m = matrix[0].size();
        int low = 0;
        int high = (n*m) -1;
        while(low<= high){
            int mid = low + (high-low)/2;
            if(matrix[mid/m][mid%m] == target){
                return true;
            }
            if(matrix[mid/m][mid%m] < target){
                low = mid+1;
            }else
            high = mid-1;
        }
        return false;

    }
};
------------------------------------------------------------------------------------------------------------------------------------------------------------------
#day9 Q14 50. Pow(x, n)
Implement pow(x, n), which calculates x raised to the power n (i.e., xn).
sol:
class Solution {
public:
    double myPow(double x, int n) {
        double ans =1.0;
        long long nn = n;
        if(nn<0)
        nn *= -1;

        while(nn){
            if(nn % 2){
                ans = ans*x;
                nn = nn-1;
            }else{
                x = x*x;
                nn /= 2;
            }           
        }
         if(n<0)
            ans =(double)(1.0)/(double)(ans);
        return ans;
    }
};


Q15 169. Majority Element
Given an array nums of size n, return the majority element.

The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.
sol:
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int ansIndex = 0;
        int n= nums.size();
        int count =1;

        for(int i=1;i<n;i++){
            if(nums[i] == nums[ansIndex]){
                count++;
            }else{
                count--;
            }

            if(count == 0){
                ansIndex = i;
                count =1;
            }
        }
        return nums[ansIndex];
    }
};
------------------------------------------------------------------------------------------------------------------------------------------------------------------
#day10 Q16 229. Majority Element II
Given an integer array of size n, find all elements that appear more than ⌊ n/3 ⌋ times.
sol:
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        int n= nums.size();
        int cnt1 =0 , cnt2 =0;
        int el1 =INT_MIN , el2 = INT_MIN;

        for(int i=0;i<n;i++){
            if(cnt1==0 && el2 !=nums[i] ){
                cnt1 =1;
                el1 = nums[i];
            }
            else if(cnt2==0 && el1 !=nums[i] ){
                cnt2 =1;
                el2 = nums[i];
            }
            else if(nums[i]==el1)
            cnt1++;
            else if(nums[i]==el2)
            cnt2++;
            else{
                cnt1--; cnt2--;
            }
        }

        vector<int>ans;
        cnt1 = 0, cnt2 = 0;
    for (int i = 0; i < n; i++) {
        if (nums[i] == el1) cnt1++;
        if (nums[i] == el2) cnt2++;
    }

    int mini = int(n / 3) + 1;
    if (cnt1 >= mini) ans.push_back(el1);
    if (cnt2 >= mini) ans.push_back(el2);
    return ans;
    }
};
