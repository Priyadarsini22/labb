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

