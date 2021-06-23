# Mpich

```go
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char** argv) {
  // Initialize the MPI environment
  MPI_Init(NULL, NULL);
  // Find out rank, size
  int world_rank;
  MPI_Comm_rank(MPI_COMM_WORLD, &world_rank);
  int world_size;
  MPI_Comm_size(MPI_COMM_WORLD, &world_size);

  // We are assuming at least 3 processes for this task
  if (world_size < 3) {
    fprintf(stderr, "World size must be greater than 2 for %s\n", argv[0]);
    MPI_Abort(MPI_COMM_WORLD, 1);
  }

  int input[10] = {123, 312 , 5, 7, 34, 95, 83, 292, 199, 21};
  if (world_rank == 0) {
    //provide data for world1
    int a[5];
    for(int i=0; i< 5; i++) {
      a[i] = input[i];
    }
    //send the first half to world1
    MPI_Send(a, 5, MPI_INT, 1, 0, MPI_COMM_WORLD);
    
    //provide data for world2
    int b[5];
    for(int i=0; i<5; i++) {
      b[i] = input[i+5];
    }
    //send the second half to world2
    MPI_Send(b, 5, MPI_INT, 2, 0, MPI_COMM_WORLD);

    //recieve world1 result
    int world1_result[5];
    MPI_Recv(world1_result, 5, MPI_INT, 1, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);

    //recieve world2 result
    int world2_result[5];
    MPI_Recv(world2_result, 5, MPI_INT, 2, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);

    // Merge the arrays
    int result[10];
    int m=0;
    int n=0;
    int k=0;
    while(k<10){
      int min;
      if (n >= 5 || (m < 5 && world2_result[n] > world1_result[m])){
        min = world1_result[m];
				m++;
      } else {
				min = world2_result[n];
        n++;
      }

      result[k] = min;
      k++;
    }

    // Print the result
    printf("Result-> sorted array is: ");
    for(int i=0; i<10; i++){
      printf("%d ", result[i]);
    }
    printf("\n");

  } else {
    //recieve my_data from world0
    int my_data[5];
    MPI_Recv(my_data, 5, MPI_INT, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);

    //calculations
    int my_result = 0;
    for(int i=0; i<5; i++) {
      my_result += my_data[i];
    }

    // sort the data
    int sorted_data[5];
    for(int i=0; i<5; i++) {
      sorted_data[i] = my_data[i];
    }
    for(int i=0; i<5; i++){
      for(int j=i; j<5; j++){
        if (sorted_data[i] > sorted_data[j]) {
				  int temp;
				  temp = sorted_data[i];
				  sorted_data[i] = sorted_data[j];
				  sorted_data[j] = temp;
				}
      }
    }

    printf("I\'m world%d, sum is: %d\n", world_rank, my_result);

    // Print the array
    printf("Sorted array in world%d is ", world_rank);
    for(int i=0; i<5; i++){
      printf("%d ", sorted_data[i]);
    }
    printf("\n");

    
    MPI_Send(sorted_data, 5, MPI_INT, 0, 0, MPI_COMM_WORLD);
  }
  MPI_Finalize();
}
```