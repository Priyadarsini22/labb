#include <stdio.h>
#include <time.h>

struct edge {
    int u, v, cost;
};
typedef struct edge edge;

int find(int v, int parent[], int n) {
    if (v < 0 || v >= n) return -1; // Check bounds
    while (parent[v] != v) {
        v = parent[v];
    }
    return v;
}

void unionij(int i, int j, int parent[], int n) {
    if (i < 0 || i >= n || j < 0 || j >= n) return; // Check bounds
    if (i < j)
        parent[j] = i;
    else
        parent[i] = j;
}

void kruskal(int n, edge e[], int m) {
    int count = 0, k = 0, sum = 0, u, v, i, j, p;
    int t[20][2], parent[20]; // Ensure these arrays are large enough
    edge temp;

    // Sort the edges based on the cost
    for (i = 0; i < m; i++) {
        for (j = 0; j < m - 1; j++) {
            if (e[j].cost > e[j + 1].cost) {
                temp = e[j];
                e[j] = e[j + 1];
                e[j + 1] = temp;
            }
        }
    }

    // Initialize parent array
    for (i = 0; i < n; i++) {
        parent[i] = i;
    }

    p = 0;
    while (count != n - 1 && p < m) { // Ensure p does not exceed the number of edges
        u = e[p].u;
        v = e[p].v;
        i = find(u, parent, n);
        j = find(v, parent, n);

        if (i != j && i != -1 && j != -1) { // Ensure valid indices
            t[k][0] = u;
            t[k][1] = v;
            k++;
            count++;
            sum += e[p].cost;
            unionij(i, j, parent, n);
        }
        p++;
    }

    if (count == n - 1) {
        printf("Spanning tree exists\n");
        printf("The spanning tree is as follows:\n");
        for (i = 0; i < n - 1; i++) {
            printf("%d  %d\n", t[i][0], t[i][1]);
        }
        printf("The cost of the spanning tree is %d\n", sum);
    } else {
        printf("\nSpanning tree does not exist\n");
    }
}

int main() {
    int n, m, a, b, i, cost;
    double clk;
    clock_t starttime, endtime;
    edge e[20];

    printf("Enter the number of vertices: ");
    scanf("%d", &n);
    if (n > 20) { // Ensure n does not exceed array bounds
        printf("Number of vertices exceeds the limit of 20.\n");
        return 1;
    }
    
    printf("Enter the number of edges: ");
    scanf("%d", &m);
    if (m > 20) { // Ensure m does not exceed array bounds
        printf("Number of edges exceeds the limit of 20.\n");
        return 1;
    }
    
    printf("Enter the edge list (u v cost):\n");

    for (i = 0; i < m; i++) {
        scanf("%d %d %d", &a, &b, &cost);
        if (a < 0 || a >= n || b < 0 || b >= n) {
            printf("Invalid vertex input.\n");
            return 1;
        }
        e[i].u = a;
        e[i].v = b;
        e[i].cost = cost;
    }

    starttime = clock();
    kruskal(n, e, m);
    endtime = clock();

    clk = (double)(endtime - starttime) / CLOCKS_PER_SEC;
    printf("The time taken is %f seconds\n", clk);

    return 0;
}





#include<stdio.h>
#include<time.h>

#define MAX 10
#define INF 999

int choose(int dist[], int s[], int n) {
    int j = 1, min = INF, w;
    for (w = 1; w <= n; w++) {
        if (dist[w] < min && s[w] == 0) {
            min = dist[w];
            j = w;
        }
    }
    return j;
}

void spath(int v, int cost[][MAX], int dist[], int n) {
    int w, u, i, num, s[MAX];
    for (i = 1; i <= n; i++) {
        s[i] = 0;
        dist[i] = cost[v][i];
    }
    s[v] = 1;
    dist[v] = 0;

    for (num = 2; num <= n; num++) {
        u = choose(dist, s, n);
        s[u] = 1;
        for (w = 1; w <= n; w++) {
            if ((dist[u] + cost[u][w]) < dist[w] && !s[w]) {
                dist[w] = dist[u] + cost[u][w];
            }
        }
    }
}

void main() {
    int i, j, cost[MAX][MAX], dist[MAX], n, v;
    double clk;
    clock_t starttime, endtime;

    printf("\nEnter number of vertices: ");
    scanf("%d", &n);
    printf("\nEnter the cost of adjacency matrix (enter %d for infinity)\n", INF);
    for (i = 1; i <= n; i++) {
        for (j = 1; j <= n; j++) {
            scanf("%d", &cost[i][j]);
        }
    }
    printf("\nEnter the source vertex: ");
    scanf("%d", &v);

    starttime = clock();
    spath(v, cost, dist, n);
    endtime = clock();

    printf("\nShortest distance\n");
    for (i = 1; i <= n; i++) {
        if (dist[i] == INF) {
            printf("\n%d to %d = INF", v, i);
        } else {
            printf("\n%d to %d = %d", v, i, dist[i]);
        }
    }
    clk = (double)(endtime - starttime) / CLOCKS_PER_SEC;
    printf("\nThe time taken is %f seconds\n", clk);
}

















#include<stdio.h>
#include<time.h>
double clk;
clock_t starttime,endtime;

int min(int a,int b)
{
	if(a<b)
	return a;
	else
	return b;
}

void floyd(int n,int W[10][10],int D[10][10])
{
   	int i,j,k;
	for(i=0;i<n;i++)
	for(j=0;j<n;j++)
	D[i][j]=W[i][j];
	for(k=0;k<n;k++)
	{
		for(i=0;i<n;i++)
		{
			for(j=0;j<n;j++)
			{
				D[i][j]=min(D[i][j],D[i][k]+D[k][j]);
			}
		}
	}
}

void main()
{
	int i,j,n,D[10][10],W[10][10];
	printf("Enter no.of vertices: \n");
	scanf("%d",&n);
	printf("Enter the cost matrix: \n");
	for(i=0;i<n;i++)
	for(j=0;j<n;j++)
	scanf("%d",&W[i][j]);
	starttime=clock();
            floyd(n,W,D);	
endtime=clock();
clk=(double)(endtime-starttime)/CLOCKS_PER_SEC;
 printf("All pair shortest path matrix is\n");
	for(i=0;i<n;i++)
	{
		for(j=0;j<n;j++)
		{
			printf("%d\t",D[i][j]);
		}
	}
   printf("\nThe run time is %f\n",clk);
}









#include <stdio.h>
#include <time.h>

// Function declarations
int max(int x, int y);
int knap(int n, int w[], int value[], int m, int v[][10]);

int main() {
    clock_t starttime, endtime;
    double clk;
    int v[10][10], n, i, j, w[10], value[10], m, result;

    printf("Enter the number of items:");
    scanf("%d", &n);

    printf("Enter the weights of %d items:\n", n);
    for (i = 0; i < n; i++) {
        scanf("%d", &w[i]);
    }

    printf("Enter the value of %d items:\n", n);
    for (i = 0; i < n; i++) {
        scanf("%d", &value[i]);
    }

    printf("Enter the capacity of the knapsack:");
    scanf("%d", &m);

    // Initialize the table v
    for (i = 0; i <= n; i++) {
        for (j = 0; j <= m; j++) {
            v[i][j] = 0;
        }
    }

    starttime = clock();
    result = knap(n, w, value, m, v);
    endtime = clock();
    clk = (double)(endtime - starttime) / CLOCKS_PER_SEC;

    printf("Optimal solution for the knapsack problem is %d\n", v[n][m]);
    printf("Time taken: %f seconds\n", clk);

    return 0;
}

// Function to find maximum of two integers
int max(int x, int y) {
    return (x > y) ? x : y;
}

// Function to solve knapsack problem using dynamic programming
int knap(int n, int w[], int value[], int m, int v[][10]) {
    int i, j;

    for (i = 0; i <= n; i++) {
        for (j = 0; j <= m; j++) {
            if (i == 0 || j == 0)
                v[i][j] = 0;
            else if (j < w[i - 1])
                v[i][j] = v[i - 1][j];
            else
                v[i][j] = max(v[i - 1][j], value[i - 1] + v[i - 1][j - w[i - 1]]);
        }
    }

    // Print the table for solving knapsack problem
    printf("\nThe table for solving knapsack problem using dynamic programming is:\n");
    for (i = 0; i <= n; i++) {
        for (j = 0; j <= m; j++) {
            printf("%d\t", v[i][j]);
        }
        printf("\n");
    }

    return v[n][m];
}









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



