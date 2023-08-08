# striver-sde-sheet
#day 1 
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
