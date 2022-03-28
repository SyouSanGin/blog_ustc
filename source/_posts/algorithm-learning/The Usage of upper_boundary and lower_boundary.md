---
title: The Usage of upper_boundary and lower_boundary
date: 2022-3-28
tags:
    - Cpp
    - Algorithm
    - STL
categories:
    - [Algorithm learning]
---

# The prototype of the two functions
```cpp
lower_bound(_ForwardIterator __first, _ForwardIterator __last,const _Tp& __val)
upper_bound(_ForwardIterator __first, _ForwardIterator __last,const _Tp& __val)
lower_bound(_ForwardIterator __first, _ForwardIterator __last,const _Tp& __val, _Compare __comp)
upper_bound(_ForwardIterator __first, _ForwardIterator __last,const _Tp& __val, _Compare __comp)

```
The type of the return value is an address or a iterator. If you want to get the index of the value you found, subtract the initial address of the array from the return value.

# Usage (In a monotone array)
* lower_bound returns the address of the first value greater than or equal to the value $\_\_val$ .
* upper_bound returns the address of the first value strictly greater than the value $\_\_val$ in the parameters.
* If the value you want doesn't exist in the array, the both of the functions will return the same address which points to the same value -- the first number strictly greater than $\_\_val$.

**Examples**:

- If exists
```cpp
int a[] = {1,2,3,4,6,6,7};
cout << lower_bound(a,a+7,6) - a<<endl<<upper_bound(a,a+7,6)-a;
```
returns:
```cpp
4 (-> 6)
6 (-> 7)
```

- If not exists
```cpp
int a[] = {1,2,3,4,6,6,7};
cout << lower_bound(a,a+7,5) - a<<endl<<upper_bound(a,a+7,5)-a;
```
returns 
```cpp
4 (->6)
4 (->6)
```

# Skills
**To get the length of a continuous array with only one value**, you can use "(upper_boundary - a)- (lower_boundary - a)"

# Implement of the Functions
* lower_boundary
```cpp
int lb(vector<int> &data, int k)
{
	int start = 0;
	int last = data.size();
	while (start < last)
	{
		int mid = (start + last) / 2;
		if (data[mid] >= k)
		{
			last = mid;
		}
		else
		{
			start = mid + 1;
		}
	}
	return start;
}
```
If data\[mid\] < k,the first value ge than k must be in the \[mid+1,last), so we assign mid+1 to start. If not, the first value ge than k must be in the \[start,mid), so we assign mid to last. At last, start points to the first value ge than your target because when we update start, start grow from the left side and wo don't choose the value that equals the k directly which ensure we will get the lower boundary of the array.


* upper_boundary
```cpp
int ub(vector<int> &data, int k)
{
	int start = 0;
	int last = data.size();
	while (start < last)
	{
		int mid = (start + last) / 2;
		if (data[mid] <= k)
		{
			start = mid + 1;
		}
		else
		{
			last = mid;
		}
	}
	return start;
}
```
Almost same as above. But attention, here we assign mid + 1 to start when data\[mid\] <=k rather than mid. We "slide" the section 1 index more to break the symmetry and ensure getting the first value that strictly greater than k.
