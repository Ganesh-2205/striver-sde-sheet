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

#day2
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
