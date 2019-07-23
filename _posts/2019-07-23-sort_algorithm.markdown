---
layout: post
title:  "Sort algorithm"
date:   2019-07-23 00:00:00 +0000
categories: git
---


### 1.Bubble sort


```java
public static void bubbleSort(int[] arr) {
	boolean isChanged;
	int length = arr.length;
	int temp;
	do {
		isChanged = false;
		for (int i = 0; i < length - 1; i++) {
			if (arr[i] > arr[i + 1]) {
				temp = arr[i];
				arr[i] = arr[i + 1];
				arr[i + 1] = temp;
				isChanged = true;
			}
		}
	} while (isChanged);
}
```

### 2.Insert sort
```java
public void insertionSort(int[] array) {
    for (int i = 1; i < array.length; i++) {
        int k = array[i];
        int j = i - 1;
        while (j >= 0 && array[j] > k) {
            array[j + 1] = array[j];
            j--;
        }
        array[j + 1] = k;
    }
}
```

### 3.Selection sort

```java
public static void selectionSort(int[] arr) {
	int temp;
	for (int i = 0; i < arr.length; i++) {
		for (int j = i + 1; j < arr.length; j++) {
			if (arr[i] > arr[j]){
				temp = arr[i];
				arr[i] = arr[j];
				arr[j] = temp;
			}
		}
	}
}
```

### 4.Quick sort

```java
public void quickSort(int[] arr, int head, int tail) {
	if (head < tail) {
		int i = head;
		int j = tail;
		int key = arr[i];
		while (i < j) {
			while (i < j && arr[j] >= key) {
				j--;
			}
			if (i < j) {
				arr[i++] = arr[j];
			}
			while (i < j && arr[i] < key) {
				i++;
			}
			if (i < j) {
				arr[j--] = arr[i];
			}
		}
		arr[i] = key;
		quickSort(arr, head, i - 1);
		quickSort(arr, i + 1, tail);
	}
}
```

