
The CPU scheduling algorithm First Come, First Served (FCFS), also known as First In, First Out (FIFO), allocates the CPU to the processes in the order they are queued in the ready queue.

FCFS uses non-preemptive scheduling, which means that once a CPU has been assigned to a process, it stays assigned to that process until it is either terminated or may be interrupted by an I/O interrupt.

```
bhgjg
jfghsjuhg sdhjgfas
kjghjas

kghvajuhg sdhgvfsd
sgfdj
```

Shortest Remaining Time First Scheduling

Shortest Remaining Time First (SRTF) algorithm always selects the process with the least amount of time remaining to execute. Here’s how it works:

1.	Preemptive Nature: Unlike some other algorithms, SRTF can preempt the currently running process if a new process arrives with a shorter remaining time.

2.	Dynamic: It constantly re-evaluates the remaining time of processes as new ones arrive and as they are executed.

3.	Minimizes Waiting Time: By always working on the shortest task left, it reduces the average waiting time for all processes.


```

// C Program to implement SRJF CPU scheduling alforithm
#include<stdio.h>
#define SIZE 10

int main(){
    int at[SIZE],bt[SIZE],bt_copy[SIZE];
    int wt[SIZE],tat[SIZE],completion[SIZE];
    int i,j,smallest,count=0,timecnt,n,beg,temp;
    float avg=0,totalt=0,end;
 
    printf("Enter the number of Processes(Maximum %d): ", SIZE-2);
    scanf("%d",&n);
    for(i=0;i<n;i++){
        printf("Enter arrival time of process %d : ",i);
        scanf("%d",&at[i]);
        printf("Enter burst time of process %d : ",i);
        scanf("%d",&bt[i]);
        bt_copy[i]=bt[i];
    }
    bt[SIZE-1]=9999;
    temp = SIZE-1;
    printf("\nGantt Chart\n");
    printf("time start to end => process number\n");
    for(timecnt=0;count!=n;timecnt++){
        smallest=SIZE-1;
        for(i=0;i<n;i++){   
            if(at[i]<=timecnt && bt[i]<bt[smallest] && bt[i]>0 )
                smallest=i;
        }
        bt[smallest]--;
        if (temp != smallest || bt[smallest]==0){
            printf("%2d to %2d => p%d\n",beg,timecnt+1,smallest);
            beg = timecnt+1;
        }
        temp = smallest;
        if(bt[smallest]==0){
            count++;
            end=timecnt+1;
            completion[smallest] = end;
            wt[smallest] = end - at[smallest] - bt_copy[smallest];
            tat[smallest] = end - at[smallest];
        }
    }
    printf("pid     burst  arrival  waiting    completion  turnaround");
    for(i=0;i<n;i++){
        printf("\n P%d \t %2d \t %2d \t %2d \t\t %2d \t %2d",i,bt_copy[i], at[i], wt[i],completion[i],tat[i]);
        avg = avg + wt[i];
        totalt = totalt + tat[i];
    }
    printf("\n\n Total waiting time =  %f",avg);
    printf("\n Total Turnaround time = %f", totalt);
    printf("\n Average waiting time = %f",avg/n);
    printf("\n Average Turnaround time = %f",totalt/n);
    
    return 0;
}
```
