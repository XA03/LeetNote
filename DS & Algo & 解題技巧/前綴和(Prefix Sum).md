通常用來解答關於  子陣列(Subarray、子數組) 的問題

設 arr={1,2,3,4,5}  所以另建一個 dp={0,0,0,0,0};
dp[0]=arr[0],再做loop dp[i]=dp[i-1]+arr[i]  得到dp={1,3,6,10,15}
其中dp[i]代表的意義是arr從0~i-1個元素的和 也就是不同的Subarray

若要得到更多種，可以用dp[x]-dp[y]的方式去刪減Subarray的元素
例如dp[3]-dp[1]=7 代表意義為 {1,2,3,4} - {1,2} ={3,4} 

也可以使用unordered_set 和 unordered_map 來存曾經列舉過的Subarray

關於Prefix Sum各操作複雜度說明

initialization  $TC=O(n)$ 至少要走過一輪原陣列
access $TC=O(1)$ 要找子陣列值只需要透過dp[i]就好
update $TC=O(n)$ 其中一個改了全部都要改

