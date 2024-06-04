#binary search
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

// Recursive binary search function
int binarySearch(int arr[], int low, int high, int target) {
    if (high >= low) {
        int mid = low + (high - low) / 2;

        // If the element is present at the middle itself
        if (arr[mid] == target)
            return mid;

        // If the element is smaller than the middle element, then it can only be present in the left subarray
        if (arr[mid] > target)
            return binarySearch(arr, low, mid - 1, target);

        // Else the element can only be present in the right subarray
        return binarySearch(arr, mid + 1, high, target);
    }

    // Element is not present in the array
    return -1;
}

int main() {
    int size;
    printf("Enter the size of the sorted array: ");
    scanf("%d", &size);

    int arr[size];

    printf("Enter the sorted array elements:\n");
    for (int i = 0; i < size; i++) {
        scanf("%d", &arr[i]);
    }

    int target;
    printf("Enter the target element to search for: ");
    scanf("%d", &target);

    // Calculate time taken by binarySearch function
    clock_t start, end;
    double cpu_time_used;

    start = clock();
    int result = binarySearch(arr, 0, size - 1, target);
    end = clock();

    cpu_time_used = ((double)(end - start)) / CLOCKS_PER_SEC;

    if (result != -1)
        printf("Element %d found at index %d.\n", target, result);
    else
        printf("Element %d not found in the array.\n", target);

    printf("Time taken by binarySearch function: %f seconds\n", cpu_time_used);

    return 0;
}









#insertion sort
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

void insertionSort(int array[], int size) {
    for (int i = 1; i < size; i++) {
        int key = array[i];
        int j = i - 1;
        while (j >= 0 && array[j] > key) {
            array[j + 1] = array[j];
            j = j - 1;
        }
        array[j + 1] = key;
    }
}

void printArray(int array[], int size) {
    for (int i = 0; i < size; i++) {
        printf("%d ", array[i]);
    }
    printf("\n");
}

int main() {
    int size;

    srand(time(NULL));

    printf("Enter the number of elements: ");
    scanf("%d", &size);

    int data[size];
    for (int i = 0; i < size; i++) {
        data[i] = rand() % 100;
    }

    printf("Unsorted array:\n");
    printArray(data, size);

    clock_t start_time = clock();
    insertionSort(data, size);
    clock_t end_time = clock();

    double time_taken = ((double)(end_time - start_time)) / CLOCKS_PER_SEC;

    printf("Sorted array:\n");
    printArray(data, size);

    printf("Time taken to sort the array: %f seconds\n", time_taken);

    return 0;
}









#merge sort
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

// Function to merge two subarrays of arr[]
void merge(int arr[], int l, int m, int r) {
    int i, j, k;
    int n1 = m - l + 1;
    int n2 = r - m;

    // Create temporary arrays 
    int L[n1], R[n2];

    // Copy data to temporary arrays L[] and R[]
    for (i = 0; i < n1; i++)
        L[i] = arr[l + i];
    for (j = 0; j < n2; j++)
        R[j] = arr[m + 1 + j];

    // Merge the temporary arrays back into arr[l..r]
    i = 0;
    j = 0;
    k = l;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }

    // Copy the remaining elements of L[], if there are any
    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }

    // Copy the remaining elements of R[], if there are any
    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}

// Recursive function to perform merge sort
void mergeSort(int arr[], int l, int r) {
    if (l < r) {
        // Same as (l+r)/2, but avoids overflow for large l and h
        int m = l + (r - l) / 2;

        // Sort first and second halves
        mergeSort(arr, l, m);
        mergeSort(arr, m + 1, r);

        // Merge the sorted halves
        merge(arr, l, m, r);
    }
}

int main() {
    int n;
    printf("Enter the size of the array: ");
    scanf("%d", &n);

    // Generate random input
    int arr[n];
    srand(time(0));
    printf("Generated array: ");
    for (int i = 0; i < n; i++) {
        arr[i] = rand() % 1000; // Generate random numbers between 0 and 999
        printf("%d ", arr[i]);
    }
    printf("\n");

    // Calculate time taken by mergeSort function
    clock_t start, end;
    double cpu_time_used;

    start = clock();
    mergeSort(arr, 0, n - 1);
    end = clock();

    cpu_time_used = ((double)(end - start)) / CLOCKS_PER_SEC;
    printf("Sorted array: ");
    for (int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");
    printf("Time taken by mergeSort function: %f seconds\n", cpu_time_used);

    return 0;
}






#quick sort
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

// Function to swap two elements
void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

// Partition function to find the partitioning index
int partition(int arr[], int low, int high) {
    int pivot = arr[high]; // Choosing the last element as the pivot
    int i = (low - 1); // Index of the smaller element

    for (int j = low; j <= high - 1; j++) {
        // If current element is smaller than the pivot
        if (arr[j] < pivot) {
            i++; // Increment index of smaller element
            swap(&arr[i], &arr[j]);
        }
    }
    swap(&arr[i + 1], &arr[high]);
    return (i + 1);
}

// Recursive Quick Sort function
void quickSort(int arr[], int low, int high) {
    if (low < high) {
        // Find partitioning index
        int p = partition(arr, low, high);

        // Recursively sort elements before and after partition
        quickSort(arr, low, p - 1);
        quickSort(arr, p + 1, high);
    }
}

int main() {
    int size;
    printf("Enter the size of the array: ");
    scanf("%d", &size);

    int arr[size];

    // Generate random input
    srand(time(0));
    printf("Generated array: ");
    for (int i = 0; i < size; i++) {
        arr[i] = rand() % 1000; // Generate random numbers between 0 and 999
        printf("%d ", arr[i]);
    }
    printf("\n");

    // Calculate time taken by quickSort function
    clock_t start, end;
    double cpu_time_used;

    start = clock();
    quickSort(arr, 0, size - 1);
    end = clock();

    cpu_time_used = ((double)(end - start)) / CLOCKS_PER_SEC;

    printf("\nSorted array: ");
    for (int i = 0; i < size; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");
    printf("\nTime taken by quickSort function: %f seconds\n", cpu_time_used);

    return 0;
}





#heap sort
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

void heapify(int arr[], int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;

    if (left < n && arr[left] > arr[largest])
        largest = left;

    if (right < n && arr[right] > arr[largest])
        largest = right;

    if (largest != i) {
        swap(&arr[i], &arr[largest]);
        heapify(arr, n, largest);
    }
}

void heapSort(int arr[], int n) {
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);

    for (int i = n - 1; i > 0; i--) {
        swap(&arr[0], &arr[i]);
        heapify(arr, i, 0);
    }
}

int main() {
    int size;
    printf("Enter the size of the array: ");
    scanf("%d", &size);

    int arr[size];

    srand(time(0));
    printf("Generated array: ");
    for (int i = 0; i < size; i++) {
        arr[i] = rand() % 1000;
        printf("%d ", arr[i]);
    }
    printf("\n");

    clock_t start, end;
    double cpu_time_used;

    start = clock();
    heapSort(arr, size);
    end = clock();

    cpu_time_used = ((double)(end - start)) / CLOCKS_PER_SEC;

    printf("\nSorted array: ");
    for (int i = 0; i < size; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");
    printf("\nTime taken by heapSort function: %f seconds\n", cpu_time_used);

    return 0;
}

#bfs
#include<stdio.h>
#include<time.h>

void bfs(int a[10][10], int n, int source) {
    int s[10], q[10], f = 0, r = -1, t, v;
    for (v = 0; v < n; v++) {
        s[v] = 0;
    }
    q[++r] = source;
    s[source] = 1;
    while (f <= r) {
        t = q[f++];
        for (v = 0; v < n; v++) {
            if (a[t][v] == 1 && s[v] == 0) {
                q[++r] = v;
                printf("The BFS traversal is: %d %d\n", t, v);
                s[v] = 1;
            }
        }
    }
}

int main() {
    int a[10][10], n, i, j, s;
    double clk;
    clock_t starttime, endtime;
    printf("Enter the number of cities\n");
    scanf("%d", &n);
    printf("Enter the matrix representation:\n");
    for (i = 0; i < n; i++) {
        for (j = 0; j < n; j++) {
            scanf("%d", &a[i][j]);
        }
    }
    printf("Enter the source city\n");
    scanf("%d", &s);
    starttime = clock();
    bfs(a, n, s);
    endtime = clock();
    clk = (double)(endtime - starttime) / CLOCKS_PER_SEC;
    printf("\nThe run time is %f\n", clk);
    return 0;
}


#dfs
#include <stdio.h>
#include <time.h>

#define MAX 50

int n, a[MAX][MAX], visited[MAX];

void DFS(int v);

int main() {
    int v;
    double clk;
    clock_t starttime, endtime;

    printf("\n\t\t\t DEPTH FIRST SEARCH \n");
    printf("\n Enter number of Lands to be surveyed: ");
    scanf("%d", &n);

    for (int i = 0; i < n; i++) {
        visited[i] = 0;
    }

    printf("\n Enter the adjacency matrix (%d x %d) row-wise (0/1):\n", n, n);
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            scanf("%d", &a[i][j]);
        }
    }

    printf("\n Enter the starting Land number: ");
    scanf("%d", &v);
    v = v - 1;

    starttime = clock();
    printf("The DFS traversal is:\n");
    DFS(v);
    endtime = clock();

    clk = (double)(endtime - starttime) / CLOCKS_PER_SEC;

    printf("\nThe run time is %f seconds\n", clk);

    return 0;
}

void DFS(int i) {
    printf("%d ", i + 1);
    visited[i] = 1;
    for (int j = 0; j < n; j++) {
        if (a[i][j] == 1 && !visited[j]) {
            DFS(j);
        }
    }
}



