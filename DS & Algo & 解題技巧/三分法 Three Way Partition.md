Dutch national flag problem的實作演算法

將一個陣列按照一個特定值拆成三個部分 小於 等於 大於 特定值
其中注意到這是分類不是排序，所以小於和大於特定值的區間中各元素不一定是有序的

**procedure** three-way-partition(A : array of values, mid : value):
    i ← 0
    j ← 0
    k ← **size of** A - 1

    while j <= k:
        if A[j] < mid:
            swap A[i] and A[j]
            i ← i + 1
            j ← j + 1
        else if A[j] > mid:
            swap A[j] and A[k]
            k ← k - 1
        else:
            j ← j + 1

流程：遍歷時檢查目前的index(j)和中位數的大小關係
