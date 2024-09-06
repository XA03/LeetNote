二元搜尋法是一種有排序就能用來降低搜尋時間成本的方法

一個二元搜尋法需要的基本元素有，範圍，中點，目標值，回傳值


BinarySearch 二元搜尋法，非常具代表性的一種搜尋法，這個搜尋法需要容器內的元素是已經排序好的。每次我們都從中間抓一個元素

比較它和我們的搜尋目標。大於代表要往後半部找，小於表示往前半部找，等於表示找到了。好處是範圍減少得很快

Best Case:O(1)元素在正中間
Average Case:O(logn)
Worst Case:O(logn)

BinarySearch有很多用途，用來搜尋目標元素只是最簡單的用途，它還可以找第一個大於目標元素的位置(等同於告訴你整個元素中有多少個元素比它大)，或是各式各樣的題目都會用到
BinarySearch有遞迴和迭代兩種版本

  
首先從最簡單的搜尋指定目標元素開始吧


網路上有很多人分享過寫法，基本上原理都是一樣的，某些地方不一樣是因為boundary check不同而已。
還有mid=(l+r)/2有時會產生overflow 所以才會改變它的計算式。
```cpp
int BinarySearch(vector<int>arr, int target) {
    int l=0,r=arr.size()-1,mid;
    while(l<=r){
	    mid=l+(r-l)/2;
	    if(arr[mid]==target)return mid;
	    else if(arr[mid]>target)r=mid-1;
	    else l=mid+1;
    }
    return -1;
}
```

上面的程式碼由於區間為[0,arr.size()-1]，所以當今天l=r時依然是在合法範圍中，要再進行一次搜尋。
例子：arr={1,2,3,4,5} , target=1  如果while條件為l<r 在最後l=r=0時就會沒檢查而找不到答案
另外 arr={1,2,3,4,5} , target=-1 如果arr[mid]>target的條件是r=mid的話就會在l=r=0時無限迴圈

這是它的另一個版本，至於覺得哪種設計比較好取決於讀者自身
```cpp
int BinarySearch(vector<int>arr, int target) {
    int l=0,r=arr.size(),mid;
    while(l<r){
	    mid=l+(r-l)/2;
	    if(arr[mid]==target)return mid;
	    else if(arr[mid]>target)r=mid;
	    else l=mid+1;
    }
    return -1;
}
```


看完找指定值的類型後，我們可以思考一下二元搜尋還能做些甚麼，一開始我們有提到二元搜尋的組成有範圍和回傳值對吧？

既然有範圍，那我們也可以利用最終的範圍起點(l)或終點(r)來代表這個值在整個數列中的特性對吧？
例如從l開始以後的數都>=nums[l]之類的？

這就是lower_bound和upper_bound的概念。
lower_bound:回傳第一個index且arr[index]>=target
另外，由於你有第一個大於等於target的數了，所以反推可以得到最後一個小於target的數index-1
```cpp
int lower_bound(vector<int>arr,int target){
	int l=0,r=arr.size()-1,mid;
	while(l<=r){
		mid=l+(r-l)/2;
		if(arr[mid]<target)l=mid+1;
		else r=mid-1;
	}
	return l;
}
```
這種形式的lower_bound最後一定會是r=l-1 所以return l才不會out of range return出-1這種不合邏輯的答案

```cpp
int lower_bound(vector<int>arr,int target){
	int l=0,r=arr.size(),mid;
	while(l<r){
		mid=l+(r-l)/2;
		if(arr[mid]<target)l=mid+1;
		else r=mid;
	}
	return l;
}
```
像這種形式最後條件l=r 回傳哪個都可以。

upper_bound:回傳最後一個index 且 arr[index]<=target

```cpp
int upper_bound(vector<int>arr,int target){
	int l=0,r=arr.size()-1,mid;
	while(l<=r){
		mid=l+(r-l)/2;
		if(arr[mid]>target)r=mid-1;
		else l=mid+1;
	}
	return r;
}
```
注意到這時return的反而是r因為我們要找的是最後一個index所以只要mid值沒有大於target前都會推進l，反而最後要靠r來代表最後一個index了。

```cpp
int upper_bound(vector<int>arr,int target){
	int l=0,r=arr.size(),mid;
	while(l<r){
		mid=l+(r-l)/2;
		if(arr[mid]>target)r=mid;
		else l=mid+1;
	}
	return l;
}
```


注意到了嗎？如果你使用的不是C++ STL的內置函數，手刻的Binary Search函數會因為你的設計而有不同的對應寫法，而l<r這種區間代表式反而擁有更高的共通性 (你不用在意甚麼第一個最後一個)


當然除了以上用法外還有更多玩二元搜尋法的方式，例如大小關係不再是用單純的比較，或是target值隨著index而改變諸如此類的。

基本上光是lower_bound跟uppper_bound搭配一些演算法觀念就可以弄得要死要活的，作者是不斷從trace code的方式去理解運作的方式再搭配各種極端條件去區別出類似的寫法有甚麼不同，再以練習去找尋自己最熟悉的方式。如果只是明白這個觀念而沒有去理解的話，除了可惜以外，也會在面對同一個觀念時只是換了個題目就毫無頭緒的浪費時間。