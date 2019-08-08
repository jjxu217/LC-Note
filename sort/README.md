# Sort

## QuickSort\(\)

### Extra-space

```python
def quicksort(self, nums):
    if len(nums) <= 1:
        return nums
    pivot = random.choice(nums)
    lt = [v for v in nums if v < pivot]
    eq = [v for v in nums if v == pivot]
    gt = [v for v in nums if v > pivot]
    return self.quicksort(lt) + eq + self.quicksort(gt)
```

### In-Space

```python
# This function takes last element as pivot, places 
# the pivot element at its correct position in sorted 
# array, and places all smaller (smaller than pivot) 
# to left of pivot and all greater elements to right of pivot 
def partition(arr, low, high): 
	i = low		 # index of smaller element 
	pivot = arr[high]	 # pivot 

	for j in range(low, high): 
		# If current element is smaller than or equal to pivot 
		if arr[j] <= pivot: 
			# increment index of smaller element 
			arr[i], arr[j] = arr[j], arr[i] 
			i += 1

	arr[i], arr[high] = arr[high], arr[i] 
	return i 

# The main function that implements QuickSort 
# arr[] --> Array to be sorted, 
# low --> Starting index, 
# high --> Ending index 

def quickSort(arr,low,high): 
	if low == high: 
		return
	# pi is partitioning index, arr[pi] is now at right place 
	pi = partition(arr, low, high) 

	# Separately sort elements before partition and after partition 
	quickSort(arr, low, pi-1) 
	quickSort(arr, pi+1, high) 

# Driver code to test above 
arr = [10, 7, 8, 9, 1, 5] 
n = len(arr) 
quickSort(arr,0,n-1) 
print ("Sorted array is:") 
for i in range(n): 
	print ("%d" %arr[i]), 
```

## MergeSort\(\)

```python
def mergeSort(arr): 
    len(arr) <= 1: 
        return
    
    mid = len(arr) // 2 #Finding the mid of the array 
    L, R = arr[:mid], arr[mid:] # Dividing the array elements into 2 halves 

    mergeSort(L) # Sorting the first half 
    mergeSort(R) # Sorting the second half 

    i = j = k = 0
    # Copy data to temp arrays L[] and R[] 
    while i < len(L) and j < len(R): 
        if L[i] < R[j]: 
            arr[k] = L[i] 
            i+=1
        else: 
            arr[k] = R[j] 
            j+=1
        k+=1

    # Checking if any element was left 
    while i < len(L): 
        arr[k] = L[i] 
        i+=1
        k+=1

    while j < len(R): 
        arr[k] = R[j] 
        j+=1
        k+=1
```

