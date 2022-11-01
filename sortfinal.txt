#include<stdio.h>
#include<stdlib.h>
#include<time.h>
//Constantuni Sorting Interface With Running Times
// Print array function
void print_array(int array[], int size){
    for(int i=0; i<size; i++){
        printf("%d ", array[i]);
    }
    printf("\n");
}
// Swap function
void swap(int *a, int *b){
    int temp = *a;
    *a = *b;
    *b = temp;
}
// 1-Insertion sort
void insertion_sort(int array[], int size){
    for(int k=1; k<size; k++){
        int key = array[k];
        int j = k-1;
        while(key<array[j] && j>=0){
            array[j+1] = array[j];
            j--;
        }
        array[j+1] = key;
    }
}
// 2-Selection sort
void selection_sort(int array[], int size){
    for(int k=0; k<size-1; k++){
        int min = k;
        for(int i=k+1; i<size; i++){
            if(array[i]<array[min]){
                min = i;
            }
        }
        swap(&array[min], &array[k]);
    }
}
// 3-Bubble sort
void bubble_sort(int array[], int size){
    for(int k=0; k<size-1; k++){
        for(int i=0; i<size-k-1; i++){
            if(array[i+1]<array[i]){
                swap(&array[i], &array[i+1]);
            }
        }
    }
}
// 4-Merge sort
void merge(int array[], int p, int q, int r) {
    int n1 = q - p + 1;
    int n2 = r - q;
    int L[n1], M[n2];
    for (int i = 0; i < n1; i++){
        L[i] = array[p + i];
    }
    for (int j = 0; j < n2; j++){
        M[j] = array[q + 1 + j];
    }
    int i, j, k;
    i = 0;
    j = 0;
    k = p;
    while (i < n1 && j < n2) {
        if (L[i] <= M[j]) {
            array[k] = L[i];
            i++;
        }
        else {
            array[k] = M[j];
            j++;
        }
        k++;
  }
    while (i < n1) {
        array[k] = L[i];
        i++;
        k++;
  }
    while (j < n2) {
        array[k] = M[j];
        j++;
        k++;
  }
}
void merge_sort(int array[], int l, int r) {
    if (l < r) {
        int m = l + (r - l) / 2;
        merge_sort(array, l, m);
        merge_sort(array, m + 1, r);
        merge(array, l, m, r);
    }
}
// 5-Quicksort
int partition(int array[], int low, int high) {
    int pivot = array[high];
    int i = (low - 1);
    for (int j = low; j < high; j++) {
        if (array[j] <= pivot) {
            i++;
            swap(&array[i], &array[j]);
        }
    }
  swap(&array[i + 1], &array[high]);
  return (i + 1);
}
void quick_sort(int array[], int low, int high) {
    if (low < high) {
        int pi = partition(array, low, high);
        quick_sort(array, low, pi - 1);
        quick_sort(array, pi + 1, high);
    }
}
// 6-Heap sort
void heapify(int array[], int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;
    if (left < n && array[left] > array[largest])
        largest = left;
    if (right < n && array[right] > array[largest])
        largest = right;
    if (largest != i) {
        swap(&array[i], &array[largest]);
        heapify(array, n, largest);
    }
    }
void heap_sort(int array[], int n) {
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(array, n, i);
    for (int i = n - 1; i >= 0; i--) {
        swap(&array[0], &array[i]);
        heapify(array, i, 0);
    }
  }
// Main
int main(){
    int inputSize;
    int chosenSort;
    printf("Please enter the input size: ");
    scanf("%d", &inputSize);
    while(!(inputSize>0)){
        printf("Please enter a valid input size: ");
        scanf("%d", &inputSize);
    }
    int data[inputSize];
    srand(time(NULL));
    for(int i=0; i<inputSize; i++){
        data[i] = (rand()%99900)+100;
    }
    // Interface
    printf("Choose a sorting algorithm from the list below: \n1-Insertion Sort\n2-Selection Sort\n3-Bubble Sort\n4-Merge Sort\n5-Quicksort\n6-Heap Sort\n");
    void sortInterface(){
        scanf("%d", &chosenSort);
        struct timespec begin, end;
        clock_t t;
        if(chosenSort==1){
            printf("1-Insertion Sort: \n");
            t = clock();
            clock_gettime(CLOCK_MONOTONIC_RAW, &begin);
            insertion_sort(data, inputSize);
        }
        else if(chosenSort==2){
            printf("2-Selection Sort: \n");
            t = clock();
            clock_gettime(CLOCK_MONOTONIC_RAW, &begin);
            selection_sort(data, inputSize);
        }
        else if(chosenSort==3){
            printf("3-Bubble Sort: \n");
            t = clock();
            clock_gettime(CLOCK_MONOTONIC_RAW, &begin);
            bubble_sort(data, inputSize);
        }
        else if(chosenSort==4){
            t = clock();
            clock_gettime(CLOCK_MONOTONIC_RAW, &begin);
            printf("4-Merge Sort: \n");
            merge_sort(data, 0, inputSize-1);
        }
        else if(chosenSort==5){
            t = clock();
            clock_gettime(CLOCK_MONOTONIC_RAW, &begin);
            printf("5-Quicksort: \n");
            quick_sort(data, 0, inputSize-1);
        }
        else if(chosenSort==6){
            printf("6-Heap Sort: \n");
            t = clock();
            clock_gettime(CLOCK_MONOTONIC_RAW, &begin);
            heap_sort(data, inputSize);
        }
        else{
            printf("Please enter a valid number from the list: ");
            return sortInterface();
        }
        // Time taken in seconds
        t = clock()-t;
        clock_gettime(CLOCK_MONOTONIC_RAW, &end);
        double runTime = ((double)t)/CLOCKS_PER_SEC;
        //print_array(data, inputSize);
        //printf("Time taken by CPU = %f milliseconds\n", runTime*1000);
        printf("Real execution time = %f milliseconds\n",((end.tv_nsec-begin.tv_nsec)/1000000000.0+(end.tv_sec-begin.tv_sec))*1000);
    }
    sortInterface();
}