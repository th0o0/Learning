### 问题描述： ###
设S = {X1,X2,...,Xn}是一个有序的集合，且X1，X2,...,Xn表示有序集合的二叉搜索树利用二叉树的顶点存储有序集中的元素，而且具有如下的性质：存储在每个顶点中的元素X大于其左子树中任一个顶点中存储的元素，小于其右子树中任意顶点中存储的元素。

----------

对于有n个关键码的集合，可以构成的二叉搜索树有![](http://i.imgur.com/c7Uz15w.jpg) 种，如果用蛮力法求解的话，基本不可能解出，在此用动态规划的思想求解。

----------
例如：4个键A、B、C、D，其对应的概率分别为：0.1 、0.2 、0.4 、0.3将会构成14中二叉树，用蛮力法求解会非常麻烦，举例如下：
![](http://i.imgur.com/5bLmuNo.jpg)

图一的平均比较次数为：0.1×1+0.2×2+0.4×3+0.3×4 = 2.9

图二的平均比较次数为：0.1×2+0.2×1+0.4×2+0.3×3 = 2.1

可以得出结论：<font color=#0099ff size =4 >结点在二叉搜索树中的层次越深，需要比较的次数就越多，因此如果需要构造一颗最小的二叉搜索树，一般尽量把搜索概率较高的结点放在较高的层次。</font>

递推式：![](http://i.imgur.com/iNMJEJK.jpg)<font color=#FFA07A size =2. >参考《算法设计与分析基础（第二版）》	P227</font>

Java实现：

	public class OptimalTreeSearch {
	
    public static int[][] OptBST(float[] P){
        int n = P.length;        //结点个数
        float[][] result = new float[n+1][n+1];
        
        int[][] R = new int[n+1][n+1];        //表达二叉查找树形状的矩阵
        
        for(int i = 0;i < n;i++)
        {      
        	result[i][i] = 0;
			result[i][i+1] = P[i];    //填充主对角线C[i,i] = P[i]
            R[i][i+1] = i+1;            //R[i][j]表示若只构造从i到j的树，那么root是R[i][j]            
        }
        for(int d = 1;d <= n - 1;d++)    //共n-1条对角线需要填充
        {
            for(int i = 1;i <= n - d;i++)    //横坐标的范围与对角线编号d的关系
            {
                int j = i + d;        //一旦横坐标确定后，纵坐标可以用横坐标与对角线编号表示出来
                float min = Integer.MAX_VALUE;
                int root = 0;
                
                for(int k = i;k <= j;k++)
                {
                	if((result[i-1][k-1] + result[k][j]) < min){
                		min = result[i-1][k-1] + result[k][j];
                		root = k;
                	}
                }      
                R[i-1][j] = root;        //R[i][j]的值代表从i到j的最优二叉查找树的根               
                float sum = 0;
                for(int s = i-1;s < j;s++)
                	sum = sum + P[s];  //----------数组越界了
                result[i-1][j] = sum + min;
            }
        }
        System.out.println("\nOBST平均键值的比较次数为:" + result[0][n]);
        System.out.println();
        System.out.println("主表如下:");
        for(int i = 0;i < result.length;i++)
        {
            for(int j = 0;j < result.length;j++)
                System.out.print(result[i][j] + "　　");
            System.out.println();
        }                
        return R;//返回表达最优二叉排序树形状的矩阵
    }
       
    public static void main(String args[]){
        char [] input = {'A','B','C','D'};
        float [] freq  = {0.1f,0.2f,0.4f,0.3f};     
        System.out.print("输入的K值:");
        for(int m = 0; m < input.length; m++)
        	System.out.print(input[m] + " ");
        System.out.print("\nK值相应的权值为:"); 
        for(int n = 0; n < input.length; n++)
        	System.out.print(freq[n] + " ");
        int[][] R = OptBST(freq);
        System.out.println();
        System.out.println("根表如下:");
        for(int i = 0;i < R.length;i++)
        {
            for(int j = 0;j < R[i].length;j++)
                System.out.print(R[i][j] + "　　");
            System.out.println();
        }
    }
	}

效果图如下：

![](http://i.imgur.com/Y97qKiy.png)

<font color=#F08080 size =4>注意：</font>

- 为了实现书中的主表和根表的效果，数组result[n+1][n+1]和R[n+1][n+1]的大小都需要 +1。
- 其中数组P[4]会出现越界的问题，因为在计算sum的时候会出现求取P[4]的情况，在此需要处理。
- **关于最后主表的结果出现1.4000001的情况，猜测是float和double精度的原因，现在暂时还不清楚。**