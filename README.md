#heapsort
#include<stdio.h>
#include<time.h>

void heapify(int h[], int n, int i) {
    int largest = i;
    int left = 2 * i;
    int right = 2 * i + 1;

    if (left <= n && h[left] > h[largest])
        largest = left;

    if (right <= n && h[right] > h[largest])
        largest = right;

    if (largest != i) {
        int temp = h[i];
        h[i] = h[largest];
        h[largest] = temp;
        heapify(h, n, largest);
    }
}

void heapSort(int h[], int n) {
    for (int i = n / 2; i >= 1; i--)
        heapify(h, n, i);

    for (int i = n; i > 1; i--) {
        int temp = h[1];
        h[1] = h[i];
        h[i] = temp;
        heapify(h, i - 1, 1);
    }
}

int main() {
    int h[20], n;
    double clk;
    clock_t starttime, endtime;

    printf("\nEnter the number of resumes: ");
    scanf("%d", &n);

    printf("\nThe candidates' ranks are:\n");
    for (int i = 1; i <= n; i++) {
        h[i] = rand() % 100;
        printf("%d ", h[i]);
    }

    starttime = clock();
    heapSort(h, n);
    endtime = clock();
    clk = (double)(endtime - starttime) / CLOCKS_PER_SEC;

    printf("\nThe ranks in sorted order:\n");
    for (int i = 1; i <= n; i++)
        printf("%d ", h[i]);

    printf("\nThe run time is %f\n", clk);

    return 0;
}


#horspool
#include <stdio.h>
#include <string.h>

#define MAX 500
int t[MAX];

void shifttable(char p[]) {
    int i, j, m;
    m = strlen(p);
    for (i = 0; i < MAX; i++) {
        t[i] = m;
    }
    for (j = 0; j < m - 1; j++) {
        t[(unsigned char)p[j]] = m - 1 - j;
    }
}

int horspool(char src[], char p[]) {
    int i, k, m, n;
    n = strlen(src);
    m = strlen(p);
    printf("\nLength of text = %d", n);
    printf("\nLength of pattern = %d", m);
    i = m - 1;
    while (i < n) {
        k = 0;
        while ((k < m) && (p[m - 1 - k] == src[i - k])) {
            k++;
        }
        if (k == m) {
            return i - m + 1;
        } else {
            i += t[(unsigned char)src[i]];
        }
    }
    return -1;
}

int main() {
    char src[100], p[100];
    int pos;
    printf("Enter the text in which pattern is to be searched:\n");
    fgets(src, sizeof(src), stdin);
    src[strcspn(src, "\n")] = '\0'; // Remove trailing newline character
    printf("Enter the pattern to be searched:\n");
    fgets(p, sizeof(p), stdin);
    p[strcspn(p, "\n")] = '\0'; // Remove trailing newline character
    shifttable(p);
    pos = horspool(src, p);
    if (pos >= 0) {
        printf("\nThe desired pattern was found starting from position %d\n", pos + 1);
    } else {
        printf("\nThe pattern was not found in the given text\n");
    }
    return 0;
}
















# labb
#include <stdio.h>
#include <time.h>
#include <stdlib.h>

int a[500000];

void merge(int low, int mid, int high) {
    int h, i, j, k;
    int b[500000];
    h = low;
    i = low;
    j = mid + 1;

    while ((h <= mid) && (j <= high)) {
        if (a[h] <= a[j]) {
            b[i] = a[h];
            h++;
        } else {
            b[i] = a[j];
            j++;
        }
        i++;
    }

    if (h > mid) {
        for (k = j; k <= high; k++) {
            b[i] = a[k];
            i++;
        }
    } else {
        for (k = h; k <= mid; k++) {
            b[i] = a[k];
            i++;
        }
    }

    for (k = low; k <= high; k++) {
        a[k] = b[k];
    }
}

void merge_sort(int low, int high) {
    int mid;
    if (low < high) {
        mid = (low + high) / 2;
        merge_sort(low, mid);
        merge_sort(mid + 1, high);
        merge(low, mid, high);
    }
}

int main() {
    int n, i;
    double clk;
    clock_t starttime, endtime;

    printf("MERGE SORT\n");
    printf("Enter the number of employee records: ");
    scanf("%d", &n);

    srand(time(NULL)); // Seed the random number generator

    for (i = 0; i < n; i++) {
        a[i] = rand() % 100;
        printf("The Employee ID %d is: %d\n", i + 1, a[i]);
    }

    starttime = clock();
    merge_sort(0, n - 1);
    endtime = clock();

    clk = (double)(endtime - starttime) / CLOCKS_PER_SEC;

    printf("\nEmployee IDs in sorted order:\n");
    for (i = 0; i < n; i++) {
        printf("%d ", a[i]);
    }
    printf("\nThe run time is %f seconds\n", clk);

    return 0;
}



#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int partition(int a[], int low, int high) {
    int i, j, temp, pivot;
    pivot = a[low];
    i = low + 1;
    j = high;

    while (1) {
        while (i < high && a[i] <= pivot)
            i++;
        while (a[j] > pivot)
            j--;
        if (i < j) {
            temp = a[i];
            a[i] = a[j];
            a[j] = temp;
        } else {
            temp = a[j];
            a[j] = a[low];
            a[low] = temp;
            return j;
        }
    }
}

void quick_sort(int a[], int low, int high) {
    int j;
    if (low < high) {
        j = partition(a, low, high);
        quick_sort(a, low, j - 1);
        quick_sort(a, j + 1, high);
    }
}

int main() {
    int i, n, a[200000];
    double clk;
    clock_t starttime, endtime;

    printf("Enter the number of student records: \n");
    scanf("%d", &n);

    srand(time(NULL)); // Seed the random number generator

    for (i = 0; i < n; i++) {
        a[i] = rand() % 100;
        printf("The roll number %d is: %d\n", i + 1, a[i]);
    }

    starttime = clock();
    quick_sort(a, 0, n - 1);
    endtime = clock();

    clk = (double)(endtime - starttime) / CLOCKS_PER_SEC;

    printf("\nSorted roll numbers are: \n");
    for (i = 0; i < n; i++) {
        printf("%d ", a[i]);
    }
    printf("\nThe run time is %f seconds\n", clk);

    return 0;
}

