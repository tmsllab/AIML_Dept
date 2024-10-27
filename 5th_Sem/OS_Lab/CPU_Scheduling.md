
Preemptive and non-preemptive CPU scheduling are two fundamental types of process scheduling.

**Preemptive Scheduling:** This type of scheduling allows the operating system to interrupt or suspend a currently running process to start or resume another process. This ensures that higher-priority processes are executed first. Common algorithms include Round Robin and Priority Scheduling.

**Non-Preemptive Scheduling:** Once a process starts running, it cannot be interrupted until it finishes. The scheduler must wait for the current process to complete before it can start the next one. FCFS and Shortest Job Next are examples of non-preemptive algorithms.

Preemptive scheduling can lead to better performance but requires more overhead for context switching. Non-preemptive scheduling is simpler but can lead to inefficient CPU use.



The CPU scheduling algorithm First Come, First Served (FCFS), also known as First In, First Out (FIFO), allocates the CPU to the processes in the order they are queued in the ready queue.

FCFS uses non-preemptive scheduling, which means that once a CPU has been assigned to a process, it stays assigned to that process until it is either terminated or may be interrupted by an I/O interrupt.

```
/* FCFS CPU Scheduling Program in C */
#include <stdio.h>
#define SIZE 10

int main()
{
    int at[SIZE],bt[SIZE],bt_copy[SIZE];
    int wt[SIZE],tat[SIZE],completion[SIZE];
    int i,count=0,timecnt,n,beg=0;
    float avg=0,totalt=0;
 
    printf("Enter the number of Processes(Maximum %d): ", SIZE-2);
    scanf("%d",&n);
    //Input details of processes
    for(i = 0; i < n; i++){
        printf("Enter Details of Process %d \n", i);
        printf("Arrival Time: ");
        scanf("%d", &at[i]);
        printf("Burst Time  : ");
        scanf("%d", &bt[i]);
        bt_copy[i] = bt[i];
    }
    
    printf("\nGantt Chart FCFS Scheduling\n");
    printf("time start to end => process number\n");
    for(timecnt=0, i = 0; count!=n; ){
        // define the conditions
        if(at[i]<=timecnt && bt[i] > 0){
            timecnt = timecnt + bt[i];
            bt[i] = 0;
            printf("%2d to %2d => p%d\n",beg,timecnt,i);
            beg = timecnt;

            count++; //decrement the process no.
            completion[i] = timecnt;
            tat[i] = completion[i] - at[i];
            wt[i] = tat[i] - bt_copy[i];
            i=0;
        }
        else{
            i++;
        }
    }
    printf("pid   arrival  burst  completion  turnaround  waiting");
    for(i=0;i<n;i++){
        printf("\n P%d %6d %7d %9d %11d %9d",i, at[i],bt_copy[i],completion[i],tat[i], wt[i]);
        avg = avg + wt[i];
        totalt = totalt + tat[i];
    }
    printf("\n\nTotal waiting time    = %f",avg);
    printf("\nTotal Turnaround time = %f", totalt);
    printf("\nAverage Waiting Time  = %f", avg/n);
    printf("\nAvg Turnaround Time   = %f", totalt/n);
    
    return 0;
}
```
OUTPUT :

![FCFS Output](https://github.com/tmsllab/AIML_Dept/blob/main/5th_Sem/OS_Lab/img/FCFS_output.jpg)

SJF Schedule
```
/* SJF CPU Scheduling Program in C */
#include <stdio.h>
#define SIZE 10

int main()
{
    int at[SIZE],bt[SIZE],bt_copy[SIZE];
    int wt[SIZE],tat[SIZE],completion[SIZE];
    int i,count,timecnt,n,beg=0,smallest;
    float avg=0,totalt=0;
 
    printf("Enter the number of Processes: ");
    scanf("%d",&n);
    count = n;
    //Input details of processes
    for(i = 0; i < n; i++){
        printf("Enter Details of Process %d \n", i);
        printf("Arrival Time: ");
        scanf("%d", &at[i]);
        printf("Burst Time  : ");
        scanf("%d", &bt[i]);
        bt_copy[i] = bt[i];
    }
    bt[SIZE-1]=9999;
    printf("\nGantt Chart SJF Scheduling\n");
    printf("time start to end => process number\n");
    for(timecnt=0, i = 0; count!=0; ){
        // define the conditions
        smallest=SIZE-1;
        for(i=0; i<n; i++){
            if(at[i]<=timecnt && bt[i]>0 && bt[i]<bt[smallest]){
                smallest=i;
            }
        }
        timecnt = timecnt + bt[smallest];
        bt[smallest] = 0;
        printf("%2d to %2d => p%d\n",beg,timecnt,smallest);
        beg = timecnt;

        count--; //decrement the process no.
        completion[smallest] = timecnt;
        tat[smallest] = completion[smallest] - at[smallest];
        wt[smallest] = tat[smallest] - bt_copy[smallest];
    }
    printf("pid   arrival  burst  completion  turnaround  waiting");
    for(i=0;i<n;i++){
        printf("\n P%d %6d %7d %9d %11d %9d",i, at[i],bt_copy[i],completion[i],tat[i], wt[i]);
        avg = avg + wt[i];
        totalt = totalt + tat[i];
    }
    printf("\n\nTotal waiting time    = %f",avg);
    printf("\nTotal Turnaround time = %f", totalt);
    printf("\nAverage Waiting Time  = %f", avg/n);
    printf("\nAvg Turnaround Time   = %f", totalt/n);
    
    return 0;
}
```
![SJF Output](https://github.com/tmsllab/AIML_Dept/blob/main/5th_Sem/OS_Lab/img/SJF_output.jpg)


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
            tat[smallest] = completion[smallest] - at[smallest];
            wt[smallest] = tat[smallest] - bt_copy[smallest];
            
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
Priority (non-preemtive) CPU Scheduling Program in C
```
/* Priority (non-preemtive) CPU Scheduling Program in C */
#include <stdio.h>
#define SIZE 10

int main()
{
    int at[SIZE],bt[SIZE],bt_copy[SIZE],priority[SIZE];
    int wt[SIZE],tat[SIZE],completion[SIZE];
    int i,count,timecnt,n,beg=0,temp,smallest;
    float avg=0,totalt=0;
 
    printf("Enter the number of Processes(Maximum %d): ", SIZE-2);
    scanf("%d",&n);
    count = n;
    //Input details of processes
    for(int i = 0; i < n; i++){
        printf("Enter Details of Process %d \n", i);
        printf("priority value(min 0): ");
        scanf("%d", &priority[i]);
        printf("Arrival Time: ");
        scanf("%d", &at[i]);
        printf("Burst Time: ");
        scanf("%d", &bt[i]);
        bt_copy[i] = bt[i];
    }
    priority[SIZE-1]=9999;
    temp = SIZE-1;
    printf("\nGantt Chart\n");
    printf("time start to end => process number\n");
    for(timecnt=0, i = 0; count!=0; ){
        // define the conditions
        smallest=SIZE-1;
        for(i=0; i<n; i++){
            if(at[i]<=timecnt && bt[i]>0 && priority[i]<priority[smallest]){
                smallest=i;
            }
        }
        timecnt = timecnt + bt[smallest];
        bt[smallest] = 0;
        printf("%2d to %2d => p%d\n",beg,timecnt,smallest);
        beg = timecnt;

        count--; //decrement the process no.
        completion[smallest] = timecnt;
        tat[smallest] = completion[smallest] - at[smallest];
        wt[smallest] = tat[smallest] - bt_copy[smallest];
    }
    printf("pid     burst  arrival  completion  turnaround  waiting");
    for(i=0;i<n;i++){
        printf("\n P%d \t %2d \t %2d \t   %2d \t\t %2d \t %2d",i,bt_copy[i], at[i],completion[i],tat[i], wt[i]);
        avg = avg + wt[i];
        totalt = totalt + tat[i];
    }
    printf("\n\nTotal waiting time    = %f",avg);
    printf("\nTotal Turnaround time = %f", totalt);
    printf("\nAverage Waiting Time  = %f", avg/n);
    printf("\nAvg Turnaround Time   = %f", totalt/n);
    
    return 0;
}


```
Priority (preemtive) CPU Scheduling Program in C
```
/* Priority (preemtive) CPU Scheduling Program in C */
#include <stdio.h>
#define SIZE 10

int main()
{
    int at[SIZE],bt[SIZE],bt_copy[SIZE],priority[SIZE];
    int wt[SIZE],tat[SIZE],completion[SIZE];
    int i,count=0,timecnt,n,beg=0,temp,smallest;
    float avg=0,totalt=0;
 
    printf("Enter the number of Processes(Maximum %d): ", SIZE-2);
    scanf("%d",&n);
    //Input details of processes
    for(int i = 0; i < n; i++){
        printf("Enter Details of Process %d \n", i);
        printf("priority value(min 0): ");
        scanf("%d", &priority[i]);
        printf("Arrival Time: ");
        scanf("%d", &at[i]);
        printf("Burst Time: ");
        scanf("%d", &bt[i]);
        bt_copy[i] = bt[i];
    }
    priority[SIZE-1]=9999;
    temp = SIZE-1;
    printf("\nGantt Chart\n");
    printf("time start to end => process number\n");
    for(timecnt=0; count!=n; timecnt++){
        // define the conditions
        smallest=SIZE-1;
        for(i=0; i<n; i++){
            if(at[i]<=timecnt && bt[i]>0 && priority[i]<priority[smallest]){
                smallest=i;
            }
        }
        bt[smallest]--;
        if (temp != smallest || bt[smallest]==0){
            printf("%2d to %2d => p%d\n",beg,timecnt+1,smallest);
            beg = timecnt+1;
        }
        temp = smallest;
        if(bt[smallest]==0){
            count++; //decrement the process no.
            completion[smallest] = timecnt+1;
            tat[smallest] = completion[smallest] - at[smallest];
            wt[smallest] = tat[smallest] - bt_copy[smallest];
        }
    }
    printf("pid     burst  arrival  completion  turnaround  waiting");
    for(i=0;i<n;i++){
        printf("\n P%d \t %2d \t %2d \t   %2d \t\t %2d \t %2d",i,bt_copy[i], at[i],completion[i],tat[i], wt[i]);
        avg = avg + wt[i];
        totalt = totalt + tat[i];
    }
    printf("\n\nTotal waiting time    = %f",avg);
    printf("\nTotal Turnaround time = %f", totalt);
    printf("\nAverage Waiting Time  = %f", avg/n);
    printf("\nAvg Turnaround Time   = %f", totalt/n);
    
    return 0;
}

```

Round Robin Scheduling
Round Robin Scheduling is a CPU scheduling algorithm in which each process is executed for
a fixed time slot. Since the resources are snatched after the time slot, round robin is
preemptive.

```
/* Round Robin Scheduling Program in C */
#include <stdio.h>
#define SIZE 10
int main()
{
    int at[SIZE],bt[SIZE],bt_copy[SIZE];
    int wt[SIZE],tat[SIZE],completion[SIZE];
    int i,count,timecnt,n,beg=0,time_slot, flag = 0;
    float avg=0,totalt=0;
 
    printf("Enter the number of Processes: ");
    scanf("%d",&n);
    count = n;
    //Input details of processes
    for(int i = 0; i < n; i++){
        printf("Enter Details of Process %d \n", i + 1);
        printf("Arrival Time: ");
        scanf("%d", &at[i]);
        printf("Burst Time: ");
        scanf("%d", &bt[i]);
        bt_copy[i] = bt[i];
    }
    //Input time slot
    printf("Enter Time Slot:");
    scanf("%d", &time_slot);
    printf("\nGantt Chart\n");
    printf("time start to end => process number\n");
    for(timecnt=0, i = 0; count!=0; ){
        // define the conditions
        if(at[i]<=timecnt && bt_copy[i] > 0){
            if(bt_copy[i] <= time_slot){
                timecnt = timecnt + bt_copy[i];
                bt_copy[i] = 0;
                flag=1;
            }
            else{
                bt_copy[i] = bt_copy[i] - time_slot;
                timecnt = timecnt + time_slot;
            }
            printf("%2d to %2d => p%d\n",beg,timecnt,i);
            beg = timecnt;
        }
        if(bt_copy[i]==0 && flag==1){
            count--; //decrement the process no.
            completion[i] = timecnt+1;
            tat[i] = completion[i] - at[i];
            wt[i] = tat[i] - bt[i];
            flag =0;
        }
        if(i == n-1){
            i = 0;
        }
        else{
            i = i+1;
        }
    }
    printf("pid     burst  arrival  waiting    completion  turnaround");
    for(i=0;i<n;i++){
        printf("\n P%d \t %2d \t %2d \t %2d \t\t %2d \t %2d",i,bt[i], at[i], wt[i],completion[i],tat[i]);
        avg = avg + wt[i];
        totalt = totalt + tat[i];
    }
    printf("\n\nTotal waiting time    = %f",avg);
    printf("\nTotal Turnaround time = %f", totalt);
    printf("\nAverage Waiting Time  = %f", avg/n);
    printf("\nAvg Turnaround Time   = %f", totalt/n);
    
    return 0;
}

```
