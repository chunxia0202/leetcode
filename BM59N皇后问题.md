```java
//N皇后问题
//N皇后n*n个格子中放置n个皇后中放置n个皇后
//要求所有皇后不同行，不同列，不同斜线，不同反斜线
//可以穷举所有的摆放规则
//这里使用回溯法
//先放置第一行的皇后，下一行的皇后不能与其同行同列同斜线反斜线，找不到其满足的位置就回溯到前一行，修改前一行的位置
//若最后拜访的行数i==n,那么说明把所有的皇后都摆放成功了，所有再次回溯，再次判断其他位置为起点是否可以摆放成功
//判断下一行能否摆放成功的时候，利用三个集合来维护，上一行对应的列，斜线，反斜线集合
import java.util.*;


public class Solution {
    HashSet<Integer> col=new HashSet<>();//前一行节点的列集合
    HashSet<Integer> poslan=new HashSet<>();//前一行节点的正斜线的集合
    HashSet<Integer> neglen=new HashSet<>();//前一行节点的反斜线集合
    int res=0;
    public int Nqueen (int n) {
       dfs(0,n);
       return res;
        
    }
    private void dfs(int i,int n){
        if(i==n){
            res++;
            return;
        }
        for(int j=0;j<n;j++){
            if(col.contains(j) || poslan.contains(i-j) || neglen.contains(i+j)){
                continue;
            }
            col.add(j);
            poslan.add(i-j);
            neglen.add(i+j);
            dfs(i+1,n);
            col.remove(j);
            poslan.remove(i-j);
            neglen.remove(i+j);
        }
    }
}
```

![image-20220320100541710](C:\Users\28635\AppData\Roaming\Typora\typora-user-images\image-20220320100541710.png)