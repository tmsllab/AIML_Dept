
```C
/* C program to implement Check Sum error detection algorithm */
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define SIZE 4

void binary_sum(char s1[],char s2[],char str_sum[], int len){
    int sum,carry,total,i,c=0;
    carry =0;
    for(i=len-1;i>=0;i--){ /* perform sum operation */
        total = (s1[i]-'0') + (s2[i]-'0') + carry;
        sum = total %2;
        carry = total /2;
        str_sum[i] = sum + '0';
    }
    printf("sum string with carry: %d %s\n",carry,str_sum);
    while(carry ==1){ /* Iterate until it don't get extra carry bit */
        for(i = len-1;i>=0;i--){ /* extra carry bit add with sum result */
            if(str_sum[i]=='0'){
                str_sum[i]='1';
                carry = 0;
                break;
            }
            else{
                str_sum[i]='0';
                carry=1;
            }
        }
        printf("sum string with carry: %d %s\n",carry,str_sum);
    }
}

void complement_1s(char str0[]){ /* perform 1s complement  */
    int i,len;
    len =strlen(str0);
    for(i=0;i<len;i++){
        if(str0[i]=='0'){
            str0[i]='1';
        }
        else{
            str0[i]='0';
        }
    }
    printf("1s Complement is: %s\n",str0);
}
void receiver_side(char s1[],char s2[],char str_sum[], int len){
    char check_sum[20],check_sum_final[20];
    check_sum[len]='\0';
    check_sum_final[len]='\0';
    printf("In receiver side\n");
    binary_sum(s1,s2,check_sum,len); /* add 1st two frame */
    printf("sum of 1st two frame= %s\n",check_sum);
    binary_sum(check_sum,str_sum,check_sum_final,len); /* add 3rd frame with sum result of 1st two frame */
    printf("sum of all 3 frames= %s\n",check_sum_final);
    complement_1s(check_sum_final);
    printf("Result=%s",check_sum_final);
    
    
}

int main()
{
    char a[20],b[20],result[20];
    int len, i,sum,carry;
    printf("Enter 1st string: ");
    scanf("%s",a);
    printf("Enter 2nd string: ");
    scanf("%s",b);
    printf("\nIn Sender side");
    printf("\n%s  %s\n",a,b);
    len = strlen(a);
    result[len]='\0';
    binary_sum(a,b,result,len);
    complement_1s(result);
    receiver_side(a,b,result,len);

    return 0;
}
```
