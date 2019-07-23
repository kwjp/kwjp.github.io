---
layout: post
title:  "Sort algorithm"
date:   2019-07-23 00:00:00 +0000
categories: git
---


### 1.Bubble sort


```sh
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




