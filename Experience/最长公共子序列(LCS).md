最长公共子序列**(LCS)**：一个数列S，若分别是2个或多个已知序列的子序列，而且是所有符合条件序列中最长的，则S称为已知序列的最长公共子序列。

比如：1 2 3 4 5 6 和 3 4 5 8 9 的LCS为：3 4 5

**最长公共子串**（Longest Common Substirng）和**最长公共子序列**（Longest Common Subsequence，LCS）的区别为：子串是串的一个连续的部分，子序列则是从不改变序列的顺序，而从序列中去掉任意的元素而获得新的序列；也就是说，子串中字符的位置必须是连续的，子序列则可以不必连续。

记： 

Xi = <x1,...,xi>(1 ≤ i ≤ m)

Yj = <y1,...,yj>(1 ≤ j ≤ n)

假定 Z = <z1,...,zk> ∈ LCS(X,Y)

- 若 xm = yn(两个序列的最后一个字符相同)则zk = xm = yn,且zk-1∈LCS(Xm-1,Yn-1),所以LCS(X,Y) = LCS(Xm-1,Yn-1)
- 若 xm ≠ yn,那么Z∈LCS(Xm-1, Y)，或者Z∈LCS(X , Yn-1)。所以LCS(X , Y)的长度为：max{LCS(Xm-1 , Y)的长度, LCS(X , Yn-1)的长度}。

即如下面公式所示：
![](http://i.imgur.com/qbYGFKa.png)

对应的代码如下：

	public class LCS{
	public int longestCommonSubsequence(String A,String B){
		if(A == null || B == null)
			return 0;
		int lenA = A.length();
		int lenB = B.length();
		int [][] lcs = new int [lenA + 1][lenB + 1];
		for(int i = lenA - 1;i >= 0;i--){
			for(int j = lenB - 1;j >= 0;j--){
				if(A.charAt(i) == B.charAt(j)){
					lcs[i][j] = 1 + lcs[i +1][j + 1];
				}else{
					lcs[i][j] = Math.max(lcs[i + 1][j],lcs[i][j +1]);
				}				
			}
		}
		return lcs[0][0];	
	}

**这个代码是从前往后推的**

输出最长公共子序列，从下图的例子可知：
![](http://i.imgur.com/jmmnNXu.jpg)

对应的代码如下：

	System.out.println("最长公共子序列为：");
		int i = 0,j = 0;
		while(i < lenA && j < lenB){
			if(A.charAt(i) == B.charAt(j)){
				System.out.print(A.charAt(i));
				i++;
				j++;
			}
			else if(lcs[i + 1][j] >= lcs[i][j + 1])
				i++;
			else 
				j++;
		}
完整的代码如下：

	public class LCS{
	public int longestCommonSubsequence(String A,String B){
		if(A == null || B == null)
			return 0;
		int lenA = A.length();
		int lenB = B.length();
		int [][] lcs = new int [lenA + 1][lenB + 1];
		for(int i = lenA - 1;i >= 0;i--){
			for(int j = lenB - 1;j >= 0;j--){
				if(A.charAt(i) == B.charAt(j)){
					lcs[i][j] = 1 + lcs[i +1][j + 1];
				}else{
					lcs[i][j] = Math.max(lcs[i + 1][j],lcs[i][j +1]);
				}	
				//System.out.println(lcs[i][j]);存储数的过程
			}
		}
		System.out.println("最长公共子序列为：");
		int i = 0,j = 0;
		while(i < lenA && j < lenB){
			if(A.charAt(i) == B.charAt(j)){
				System.out.print(A.charAt(i));
				i++;
				j++;
			}
			else if(lcs[i + 1][j] >= lcs[i][j + 1])
				i++;
			else 
				j++;
		}
		return lcs[0][0];	
	}
	
	public static void main(String[] args) {
		LCS lCS = new LCS();
		String A = "BDCABA";
		String B = "ABCBDAB";
		System.out.println("字符串A为：" + A);
		System.out.println("字符串B为：" + B);
		int temp = lCS.longestCommonSubsequence(A,B);
		System.out.println("\n子序列的长度为：" + temp);
		}		
	}


**注：**利用到来动态规划的知识，现在在此留坑，以后再来详细谈谈这个题目 解法。

2/16/2017 10:28:45 AM 