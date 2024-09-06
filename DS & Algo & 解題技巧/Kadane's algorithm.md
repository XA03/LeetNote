
從一個陣列中找出最大子陣列的的方法

假設一個例子{a,b,c}

子陣列有 {a} {b} {c} {a,b} {b,c} {a,b,c} 六種

從開頭往後找最大子陣列時，第一步因為還沒有取到任何資訊所以先取{a}

接著要判斷有沒有更大的子陣列時，判斷的是{b},{a,b}哪個比較大

如果 b>a,b 代表 a是負的 所以我們應該要把a丟掉 也就是最大值變{b}

如果b=a,b 代表b=0 不影響結果

如果b<a,b 代表b是負的 所以我們最大值還是 {a}

由此可知要找最大值的利用dp[i]=max(dp[i-1]+nums[i],nums[i]) 就好

接著考慮如何找出這個最大值代表的陣列

我的方法是利用捨棄前方答案的時候定位最大值陣列的開始座標

接著利用找出的最大值從開始座標向後遍歷dp值直到最大值就找到結束座標

```cpp
#include <bits/stdc++.h>
#include <iostream>
using namespace std;
int main(){

    vector<int>arr={2,2,-9,4};

    vector<int>dp;

    dp.push_back(arr[0]);
    int l=0,r=0;


    for(int i=1;i<arr.size();i++){
        if(dp[i-1]+arr[i]>arr[i]){
            dp.push_back(dp[i-1]+arr[i]);
        }

        else{
            dp.push_back(arr[i]);
            l=i;
            r=i;
        }
    }

    for(int i=l;i<dp.size();i++){
        if(dp[i]==*max_element(dp.begin(),dp.end()))break;
        r++;
    }
    vector<int>ans(arr.begin()+l,arr.begin()+r+1);
    for(auto i:ans)cout<<i<<" ";
  
    return 0;
}

```


作者的方法有個瑕疵是 當今天最大值陣列有多個的時候只會找到最後一個

也許之所以不會要作答者輸出最大陣列而是值就是因為如此吧，陣列內容不唯一但值唯一