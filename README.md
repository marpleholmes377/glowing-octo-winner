#include <stdlib.h>
#include <stdio.h>
#include <time.h>
#include <string.h>
#include<stdbool.h>

#define MAX 100000000
#define MIN -100000000
bool s[MAX] = { 0 };

// interpolation search implementation
void interpolationSearch(int arr[], int arr_size, int sum)
{
    int i, temp;

    /*initialize hash set as 0*/
    
 
    for (i = 0; i < arr_size; i++)
    {
        temp = sum - arr[i];
        if (s[temp] == 1){
            printf("The 2-sum combination is %d + %d = %d\n",arr[i],temp, sum);
            break;
        }
        s[arr[i]] = 1;
    }
}

// binary search implementation
void binarySearch(int array[], int target, int arr_size){
    int left = 0;
    int right = arr_size-1;
    while(left < right) {
      int sum = array[left] + array[right];
      if(sum == target) {
        printf("The 2-sum combination is %d + %d = %d\n",array[left],array[right], target);
        break;
      } else if (sum < target) {
        left++;
      } else{
        right--;
      }
    }
}

// helper function to swap in selection sort
void swap(int* xp, int* yp)
{
    int temp = *xp;
    *xp = *yp;
    *yp = temp;
}
 
// Function to perform Selection Sort
int * selectionSort(int arr[], int n)
{
    int i, j, min_idx;
 
    // One by one move boundary of unsorted subarray
    for (i = 0; i < n - 1; i++) {
 
        // Find the minimum element in unsorted array
        min_idx = i;
        for (j = i + 1; j < n; j++)
            if (arr[j] < arr[min_idx])
                min_idx = j;
 
        // Swap the found minimum element
        // with the first element
        swap(&arr[min_idx], &arr[i]);
    }

    return arr;
}

// linear search implementation
void linearSearch(int arr[], int n, int sum)
{
    // Consider all possible pairs and check
    // their sums
    int found = 0;
    for (int i = 0; i < n; i++){
        for (int j = i + 1; j < n; j++){
            if (arr[i] + arr[j] == sum){
                printf("The 2-sum combination is %d + %d = %d\n",arr[i],arr[j],sum);
                found = 1;
            }
        }
        if (found == 1)
        {
            break;
        }
    }
}

int main()   // define the main function  
{  
    char choice = 'Y';

    while (choice == 'Y')
    {

        int array_size = 0;
        // Step 1. Generate random numbers 1-100 and store into txt file
        printf("Enter the size of array(100,200,300)");
        scanf("%d",&array_size);
        int random_numbers[array_size];
        int lower = MIN, upper = MAX, count = array_size;

        srand(time(0));

        // Open file
        FILE *fptr;
        // fptr = fopen("numbers.csv","w");
        // if(fptr == NULL)
        // {
        //     printf("Error");   
        //     exit(1);             
        // }
        // // Write numbers into file
        // for (int i = 0; i < count; i++) {
        //     int num = (rand() % (upper - lower + 1)) + lower;
        //     fprintf(fptr,"%d\n",num);
        // }
        // fclose(fptr);

        // Step 2. read from file and store in array
        fptr = fopen("numbers.csv", "r");

        if (fptr == NULL)
        {
            printf("Error: could not open file numbers.csv");
            return 1;
        }

        // reading file and store into array
        for (int i = 0; i < array_size; i++)
        {
            fscanf(fptr, "%d", &random_numbers[i]);
        }
        fclose(fptr);

        // for (int i = 0; i < count; i++) {
        //     printf("%d\n",random_numbers[i]);
        // }

        // Menu
        int i = 0;
        int sum_target = 0;
        printf("Menu\n");  
        printf("1. Linear Search\n");
        printf("2. Binary Search\n");  
        printf("3. Interpolation Search\n"); 
        printf("Choose a search: "); 
        scanf("%d", &i);

        clock_t t;
        double time_taken = 0.00;

        switch(i)
        {
            case 1:
                printf("Array = ");

                for (int i = 0; i < count; i++) {
                    printf("%d,",random_numbers[i]);
                }
                printf("\n");
                printf("Input target sum = "); 
                scanf("%d", &sum_target);

                t = clock();
                linearSearch(random_numbers, array_size, sum_target);
                t = clock() - t;
                time_taken = ((double)t)/CLOCKS_PER_SEC; // in seconds
                printf("The runtime / execution time of Linear search is %f millisecond.  \n", time_taken*1000);
                break;

            case 2:
                printf("Array = ");

                for (int i = 0; i < count; i++) {
                    printf("%d,",random_numbers[i]);
                }
                printf("\n");
                printf("Input target sum = "); 
                scanf("%d", &sum_target);

                t = clock();
                int *sortedArray;
                sortedArray = selectionSort(random_numbers, array_size);
                binarySearch(sortedArray, sum_target, array_size);
                t = clock() - t;
                time_taken = ((double)t)/CLOCKS_PER_SEC; // in seconds
                printf("The runtime / execution time of Binary search is %f millisecond.  \n", time_taken*1000);
                break;

            case 3:
                printf("Array = ");

                for (int i = 0; i < count; i++) {
                    printf("%d,",random_numbers[i]);
                }
                printf("\n");
                printf("Input target sum = "); 
                scanf("%d", &sum_target);

                t = clock();
                interpolationSearch(random_numbers, array_size, sum_target);
                getchar();
                t = clock() - t;
                time_taken = ((double)t)/CLOCKS_PER_SEC; // in seconds
                printf("The runtime / execution time of Interpolation search is %f millisecond.  \n", time_taken*1000);
                break;

            // operator doesn't match any case constant +, -, *, /
            default:
                printf("No selection found");
        }

        printf("Do you want to search again? (Y/N)");
        scanf(" %c", &choice);
    }

    return 0;
}  
