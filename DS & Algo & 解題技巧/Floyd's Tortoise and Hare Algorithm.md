
Floyd's Cycle detection ，可以在有限狀態機、疊代函數、鏈表(Linked List)判斷是否存在環、找出環的起點、環的長度

我們將一個含有Cycle的Linked List分成 不是Cycle 和 Cycle兩個部分

不是Cycle的部分我們假設長度為T，Cycle的長度為C。

我們可以根據餘式定理寫出  T=k * C+R (0<=R<C)

接著我們設兩個Pointer，slow和fast。

slow一次走一步，fast一次走兩步。

假設Linked List起點的節點是-T，則Cycle起點為0

經過T次移動後，slow在0而fast在 -T+2T=T=k * C + R.

很明顯 fast此時已經在Cycle裡了，加上Cycle的起點是0，因此fast的位置應該要mod C

也就是說fast=R.

接著再移動C-R次

slow的位置是C-R fast的位置是 2C-R mod C = C-R 得證只要有環，兩個不同速的Pointer必定會相遇。

接著要找出環的起點，我們將slow移回起點-T。

移動T次，slow依然回到0，而這時fast=(C-R)+k * C +R= (k-1) C mod C =0  再次相遇

因此當第一次相遇後將慢的Pointer移回起點，Pointer再次相遇的點即為環的起點。


接著討論環的長度，當第一次相遇後讓slow，fast繼續走下去的話，它們第二次相遇時fast走的步數為一圈。


Floyd's Cycle detection 還可以用來判斷兩個Linked List是否相交。

首先來看Linked List 相交的定義是甚麼，兩個Linked List相交代表著這兩條Linked List從某個節點後的節點是共用的，就像Y字形一樣原先分開而後合併。

如何判斷呢？我們可以先把A Linked List的尾巴接著B Linked List的頭，如果A跟B原本是相交的，那接完兩條Linked List後就會變成一條有環的Linked List。 你把Y的尾巴接到其中一個
開頭，就產生了環。

反之，一開始沒有相交，就算頭接尾後也不會產生環。