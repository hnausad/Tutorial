## 1. Bubble Sort
- void bubbleSort(int[] arr) {
-    int n = arr.length;
-    for (int i = 0; i < n - 1; i++) {
-        for (int j = 0; j < n - i - 1; j++) {
-            if (arr[j] > arr[j + 1]) {
-                int temp = arr[j];
-                arr[j] = arr[j + 1];
-                arr[j + 1] = temp;
-           }
-        }
-    }
- }
**Best Case: O(n)**

**Worst Case: O(n²)**

**Use Case: Small or mostly sorted lists.**

---
## 2. Selection Sort
void selectionSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        int minIndex = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }
        int temp = arr[minIndex];
        arr[minIndex] = arr[i];
        arr[i] = temp;
    }
}
Time: O(n²)

Use Case: When memory writes are costly.

---
## 3. Insertion Sort
void insertionSort(int[] arr) {
    for (int i = 1; i < arr.length; i++) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}

Best Case: O(n)

Worst Case: O(n²)

Use Case: Small data or nearly sorted arrays.

---

## 4. Merge Sort
void mergeSort(int[] arr, int left, int right) {
    if (left < right) {
        int mid = (left + right) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}

void merge(int[] arr, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;

    int[] L = new int[n1];
    int[] R = new int[n2];

    for (int i = 0; i < n1; i++) L[i] = arr[left + i];
    for (int j = 0; j < n2; j++) R[j] = arr[mid + 1 + j];

    int i = 0, j = 0, k = left;

    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) arr[k++] = L[i++];
        else arr[k++] = R[j++];
    }

    while (i < n1) arr[k++] = L[i++];
    while (j < n2) arr[k++] = R[j++];
}

Time: O(n log n)

Use Case: Large datasets, stable sort needed.

---
## 5. Quick Sort
void quickSort(int[] arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

int partition(int[] arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            int temp = arr[i]; arr[i] = arr[j]; arr[j] = temp;
        }
    }
    int temp = arr[i + 1]; arr[i + 1] = arr[high]; arr[high] = temp;
    return i + 1;
}
Worst Case: O(n²) (rare)

Use Case: Fastest in practice for large arrays.

---

## 6. Heap Sort
void heapSort(int[] arr) {
    int n = arr.length;

    for (int i = n / 2 - 1; i >= 0; i--) heapify(arr, n, i);

    for (int i = n - 1; i >= 0; i--) {
        int temp = arr[0]; arr[0] = arr[i]; arr[i] = temp;
        heapify(arr, i, 0);
    }
}

void heapify(int[] arr, int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;

    if (left < n && arr[left] > arr[largest]) largest = left;
    if (right < n && arr[right] > arr[largest]) largest = right;

    if (largest != i) {
        int swap = arr[i];
        arr[i] = arr[largest];
        arr[largest] = swap;
        heapify(arr, n, largest);
    }
}
Time: O(n log n)

Use Case: In-place sort, not stable.

---

 ## 7. Counting Sort
void countingSort(int[] arr) {
    int max = Arrays.stream(arr).max().getAsInt();
    int[] count = new int[max + 1];
    
    for (int num : arr) count[num]++;
    
    int index = 0;
    for (int i = 0; i < count.length; i++) {
        while (count[i]-- > 0) arr[index++] = i;
    }
}
Time: O(n + k)

Use Case: Sorting integers within a fixed small range.

---

## 8. Radix Sort (for integers)
void radixSort(int[] arr) {
    int max = Arrays.stream(arr).max().getAsInt();
    for (int exp = 1; max / exp > 0; exp *= 10) {
        countingSortByDigit(arr, exp);
    }
}

void countingSortByDigit(int[] arr, int exp) {
    int n = arr.length;
    int[] output = new int[n];
    int[] count = new int[10];

    for (int i = 0; i < n; i++) count[(arr[i] / exp) % 10]++;
    for (int i = 1; i < 10; i++) count[i] += count[i - 1];
    for (int i = n - 1; i >= 0; i--) {
        int digit = (arr[i] / exp) % 10;
        output[--count[digit]] = arr[i];
    }
    System.arraycopy(output, 0, arr, 0, n);
}
Time: O(nk) where k is number of digits.

Use Case: Large list of integers with uniform length.

---

## 9. Bucket Sort
void bucketSort(float[] arr) {
    int n = arr.length;
    List<Float>[] buckets = new List[n];

    for (int i = 0; i < n; i++) buckets[i] = new ArrayList<>();
    for (float num : arr) buckets[(int)(n * num)].add(num);
    for (List<Float> bucket : buckets) Collections.sort(bucket);

    int index = 0;
    for (List<Float> bucket : buckets) {
        for (float num : bucket) arr[index++] = num;
    }
}
Time: O(n + k)

Use Case: Uniformly distributed floating point numbers.

---

## 10. Shell Sort
void shellSort(int[] arr) {
    int n = arr.length;
    for (int gap = n / 2; gap > 0; gap /= 2) {
        for (int i = gap; i < n; i++) {
            int temp = arr[i];
            int j = i;
            while (j >= gap && arr[j - gap] > temp) {
                arr[j] = arr[j - gap];
                j -= gap;
            }
            arr[j] = temp;
        }
    }
}
Time: O(n log n) (depends on gap sequence)

Use Case: Better than insertion sort for medium-sized arrays.
