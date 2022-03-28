---
title: Binary search template
date: 2022-3-28
tags:
    - Cpp
    - Algorithm
categories:
    - [Algorithm learning]
---
# Recursive Solution

```cpp
int binary_search(T* arr, int st, int fn, T  tg)
{
	if (st == fn) return arr[st] == tg ? st:-1;
    
   	int mid = st + fn >> 1;
   	if (arr[mid] == tg) return mid;
   	else if (arr[mid] < tg) return binary_search(arr,mid+1,fn,tg);
   	else return binary_search(arr,st,mid-1,tg);
}
```
# Non-recursive Solution

## Greneral
```cpp
int find(int x , int n, int * arr)
{
    if (arr[1]>x || arr[n]<x) return -1; // To exclude the x outside of [min,max] to avoid bugs
    int l = 1,r = n,ans = -1145141919;
    while (l<=r)
        {
            int mid = l + r >>2;
            if (arr[mid]<=x) ans = mid , l = mid + 1;
            else l = mid -1;
        }
    return arr[ans] == x?ans:-1; // To ensure the result is the correct value
}
```
This method returns the lower boundary of the array that contains the target number, because it increase from the left side. If we change the condition into arr\[mid\] >= x,we can get the upper boundary of the array contains the target number.
## Pay attention to the two types below
**Type 1**
```cpp
	while (l < r){
		int mid = l+r>>1;
		if (tar <= arr[mid]) r = mid ;
		else l = mid + 1;
	}
```
**Type 2**
```cpp
	while (l < r){
		int mid = (l+r >> 1) + 1;
		if (tar >= mid) l = mid;
		else r = mid -1;		
	}
```
The difference between them is whether we choose $floor(\frac{l+r}{2})$ or $ceil(\frac{l+r}{2})$. If we choose the former one, when it comes to the condition that $r-l=1$, we may find that $mid$ equals $l$ and the loop becomes an infinite loop. So at this time, we choose to make $r = mid$ when $tar\le arr\left [mid 
\right ]$ beacuse the variate "r" never equals "mid" and at that time we need to update the variate "r". And then we ensure that the loop won't be an infinite loop. The Type 2 is almost the same as Type 1.

If the number you're looking for isn't exists in the array, the first type will return the index of the last number that less than the target while the second one returns the index of the first number greater than it.

# ATTENTION
The constant of the time of the recursive solution is much greater than the non-recursive one. If the data is within 1e5 ,the former one is safe but when it comes to 1e6 and larger, TLE may occor. Try to use non-recursive one if possible.
