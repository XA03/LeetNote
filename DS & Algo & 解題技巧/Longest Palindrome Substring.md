在一個字串中找出最長的迴文子字串。

迴文：從前往後看和從後往前看是一樣的字串

例子：aba,abba 是迴文，abcd,acac不是


解1. BruteForce

從一個字串中找出Longest Palindrome Substring的暴力法是選取字串中的各個位置當做迴文的中心點並向左右比較。

當遇到偶數長度的迴文時，我們可以透過改變原本字串的格式來使每次找到的字串都是奇數長度

方法如下，給字串頭尾和各字元間加上區隔符號可以使每次找到的字串變成奇數

例子：aaa→!a!a!a! aa→!a!a! 如此一來就可以避免偶數長度時無法定位中心點的問題

最後取出字串時再將區隔符號移除就好。

$TC=O(n^2)$
```cpp
#include <bits/stdc++.h>

#include <iostream>

using namespace std;

  
void repstr(string &str){
    string temp="$";
    for(int i=0;i<str.size();i++){
        temp+=str[i];
        temp+="$";
    }
    str=temp;
}

void cleanstr(string &str){
    string temp;
    for(int i=0;i<str.size();i++){
        if(str[i]=='$')continue;
        temp+=str[i];
    }
    str=temp;
}

int main(){  
    string str="ababacccc";
    repstr(str);
    vector<int>times(str.size(),1);
    vector<string>palin(str.size(),"");

    for(int i=0;i<str.size();i++){
        int radius=0;
        while(i-(radius+1)>=0 && i+(radius+1)<str.size() && str[i-(radius+1)]==str[i+(radius+1)])radius++;
        times[i]=radius;
    }
    for(int i=1;i<times.size();i++)cout<<times[i]<<" ";
    for(int i=0;i<palin.size();i++){
        string palindrome=str.substr(i-times[i],2*times[i]+1);
        cleanstr(palindrome);
        palin[i]=palindrome;
    }
    cout<<"\n\n";
    for(int i=1;i<times.size();i++)cout<<palin[i]<<" ";
    return 0;
}
```

解2. Manachar's algorithm 

以中心擴展算法為基礎加上Dynamic Programming概念去除不必要的檢查。

會產生幾種情況，某些迴文串中會有子迴文而這些子迴文可以透過鏡射的技巧去對應到目前位置後的字串。

例如：xabayabad 以y為中心左側的子迴文是aba，而這個aba可以對應到大迴文的左邊界aba
探討的情況是這個子迴文的左邊界和整個迴文左邊之間的關係。

以xabayabad為例，大迴文為abayaba，其中子迴文為aba。因為子迴文左邊界等於大迴文的左邊界，所以這個字串是有可能繼續延伸下去的 (即使此例中x!=d) 
這種情況才需要用中心擴展方法去尋找可能存在的最大迴文串。


以xabajabax為例，大迴文為xabajabax，子迴文為aba。子迴文左邊界並沒有超出大迴文的左邊界(xaba)，因此即使鏡射過後也不會超過大迴文串的長度。所以就可以跳過這種情況

以ababajababk為例，大迴文為babajabab，但子迴文為ababa。因此子迴文ababa超出了大迴文的左邊界baba。因此即使鏡射也不可能和大迴文後續的字元對應，不會產生新的最長迴文串，因此也能跳過。
就此例而言，若ababa可以鏡射到j的右方成為新串，那大迴文就不可能是babajabab(因為a!=k)。

在中心擴展的基礎加上dp表格紀錄前面找到以i為中心的最長迴文子字串長度。

center是當前大迴文串中心點，mirrori是當前位置的鏡射前位置，right是大迴文串右邊界
distance是右邊界與當前位置的距離。

若遇到的字元是分隔符直接增加半徑就好，透過適當的處理不影響後續輸出答案。
當前位置+上半徑超過右邊界時就更新新的中心點位置和半徑。

Q&A：

Q1:mirroi的作用？ A:為了利用前面dp表格的結果，所以反向以中心點鏡射前面的位置
Q2:distance的作用？ A:用來快速計算鏡像位置的最大迴文子串長度的值，可以減少檢查的次數
```cpp
string Manacher(string s) {
        int center=1,right=1,maxi=0,maxlen=1;
        vector<int>dp(2*s.size()+1,0);
        for(int i=1;i<dp.size();i++){
            int mirrori=center-(i-center),distance=right-i;
            if(distance>0)dp[i]=min(dp[mirrori],distance);
            while(i-dp[i]>0 && i+dp[i]<dp.size()-1 && ((i+dp[i]+1)%2==0 || (s[(i+dp[i]+1)/2]==s[(i-dp[i]-1)/2])))dp[i]++;
            
            
            if(dp[i]>maxlen){
                maxlen=dp[i];
                maxi=i;
            }
            
            
            if(i+dp[i]>right){
                center=i;
                right=i+dp[i];
            }
            
        }
        
        int index=(maxi-maxlen)/2;
        
        return s.substr(index,maxlen);
}
```



