# Uni
Write a program in C which reads input CPU bursts from a the first  line of a text file named as CPU_BURST.txt. Validate the input numbers whether   //   the number are positive intergers or not. Consider the number as CPU burst.if there are 5 positive integer in the first line of the text file then    //  the program treat those argument as required CPU bust for P1,P2,P3,P4 and P5 process and calculate average waiting time and average turn around time     // Consider used scheduling algorithem as SJF and same arrival time for all the processes 



#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>

#define FILE_NAME "CPU_BURST.txt"

struct process{
           int at,bt,wt,tat;
           char name[4];
};

struct process initialize(int at, int bt, int name){
                       struct process X;
                       X.bt=bt;
                       X.at=at;
                       sprintf(X.name,"p%d",name+1);
                       return X;
}

int main()
	{
	       
	       FILE *fp = fopen(FILE_NAME,"r");
	       if(!fp)
	              return -1*printf("FILE OPEN ERROR!\n");
	 
	 int d,i,j,count=0;
	 
	 int *queue=(int*)malloc(sizeof(int));
	
	 while(EOF !=fscanf(fp,"%d ",&d))
	{
	      printf("%d",d);
	      queue = (int*)realloc(queue,(count+1)*sizeof(int));
	      queue[count++] =d;
	}
	fclose(fp);
	  //int queue[] ={ 3,1,3,2,4,5};
	 
	  struct process p[count];
	 
	  for(i=0; i<count; i++)
	           p[i] = initialize(0,queue[i],i);
	  
	   //sort
	   for(i=1; i<count; i++)
		{
	         for(j=0; j<count-i; j++)
			 {
	                  if(p[j].bt>p[j+1].bt)
					  	{
	                                   struct process temp = p[j];
	                                   p[j] =p[j+1];
	                                   p[j+1] =temp;
	                   }
	         }
		}
	 
	 printf("\nOrder:");
	
	 int elapsed_time=0;
	 for(i=0; i<count; i++)
	 {
	    p[i].wt =elapsed_time;
	    p[i].tat=p[i].wt+p[i].bt;
	    elapsed_time +=p[i].bt;
	    printf("%s", p[i].name);
	}
	for(i=1; i<count; i++)
	{
	       for(j=0; j<count-i; j++)
		   {
	            if(p[j].name[1]>p[j+1].name[1])
				{
	                 struct process temp=p[j];
	                 p[j] =p[j+1];
	                 p[j+1]=temp;
	        	  }
	    	}
	}
	
	printf("\n\n%7s|%8s|%6s|%5s|%s\n","PROCESS","ARRIVAL","BURST","WAIT","TURNAROUND");
	
	 int total_wt=0,total_tt=0;
	 for(i=0; i<count; i++)
	 {
	            total_wt+=p[i].wt;
	            total_tt+=p[i].tat;
	            printf("%7s|%8d|%6d|%5d|%9d\n",p[i].name,p[i].at,p[i].bt,p[i].wt,p[i].tat);
	}
	
	printf("\nAverage Waiting Time      :%.2f\n",total_wt*1.0/count);
	printf("\nAverage Turn Around Time  :%.2f\n",total_tt*1.0/count);
	
	return 0*printf("\nSUCCESSFUL EXIT\n");
}
