Program 5

Circular queue (Dynamic memory allocation)
#include <stdio.h>
#include <stdlib.h>

int QS = 4;   // initial capacity

int *insert(int q[], int *f, int *r, int *cnt);
void del(int q[], int *f, int *cnt);
void display(int q[], int f, int cnt);


int main()
{
    int *q = (int *) malloc(sizeof(int) * QS);
    if (!q)
    {
        printf("malloc failed\n");
        return 1; 
     }
    int r = -1, f = 0, cnt = 0;
    int ch;
    while (1)
    {
        printf("\nCIRCULAR QUEUE WITH DYNAMIC MEMORY\n"
               "1: Insert\n2: Delete\n3: Display\n4: Exit\n"
               "Enter choice: ");
        scanf("%d", &ch);
        
        switch (ch) 
        {
            case 1:
                q = insert(q, &f, &r, &cnt);
                break;
            case 2:
                del(q, &f, &cnt);
                break;
            case 3:
                display(q, f, cnt);
                break;
            case 4:
                free(q);
                printf("Exiting\n");
                return 0;
            default:
                printf("Invalid choice\n");
        }
    }
}

/*
 * Insert: if queue is full, allocate a new array of double capacity,
 * copy logical elements in order to new array starting at index 0,
 * update f=0, r = cnt-1, update global QS, free old array, then insert.
 */
int *insert(int q[], int *f, int *r, int *cnt)
{

    int ele;
    int *p = q;
    if ((*cnt) == QS)
    {
        int newCap = QS * 2;
        p = (int *) malloc(sizeof(int) * newCap);
        if (!p) 
        {
            printf("Memory allocation failed while resizing\n");
            return q; // return old pointer (no change)
        }
        // copy logical elements from old circular buffer into p[0..cnt-1]
        int idx = 0;
        if (*r >= *f)
        {
            // non-wrapped case: elements at f..r
            for (int i = *f; i <= *r; ++i)
                p[idx++] = q[i];
        }
        else
        {
            // wrapped case: elements f..QS-1 then 0..r
            for (int i = *f; i <= QS - 1; ++i)
                p[idx++] = q[i];
            for (int i = 0; i <= *r; ++i)
                p[idx++] = q[i];
        }
        // free old buffer and switch to new
        free(q);
        q = p;
        // reset indices to linear layout
        *f = 0;
        *r = (*cnt) - 1;   // last valid index after copy
        QS = newCap;

        printf("After doubling and rearranging (new QS = %d)\n", QS);
        for (int i = 0; i < *cnt; ++i) 
            printf("%d ", q[i]);
        printf("\n");
    }
    // now insert the new element (there is guaranteed space)
    *r = ((*r) + 1) % QS;
    printf("Enter the ele: ");
    scanf("%d", &ele);
    q[*r] = ele;
    (*cnt)++;
    printf("Inserted %d at index %d (count=%d)\n", ele, *r, *cnt);
    return q;
}
void del(int q[], int *f, int *cnt)
{
    if ((*cnt) == 0) 
    {
        printf("Queue is empty\n");
        return;
    }
    printf("Element deleted from circular queue is %d (index %d)\n", q[*f], *f);
    *f = ((*f) + 1) % QS;
    (*cnt)--;
    // optional: reset indices if empty to keep values tidy
    if (*cnt == 0) {
        *f = 0;
    }
}
void display(int q[], int f, int cnt) 
{
    if (cnt == 0) 
    {
        printf("Circular Queue is empty\n");
        return;
    }
    printf("Circular Queue contents (index : value)\n");
    int i = f;
    for (int j = 0; j < cnt; ++j)
    {
        printf("%d : %d\n", i, q[i]);
        i = (i + 1) % QS;
    }
}




program 4

# include <stdio.h>
# include <string.h>  
# include <math.h>   // for pow function
# include <ctype.h>   // for isdigit function

double compute(char symbol,double op1, double op2) 
{
  switch(symbol)
  {
  	case '+': return op1 + op2;
 	case '-': return op1 - op2;
    case '*': return op1 * op2;
	case '/': return op1 / op2;
	case '$':
	case '^': return pow(op1,op2);
  }
}

int main() 
{
 char postfix[20],symbol;
 double st[20],op1,op2;
 int top=-1,i;
 printf("Enter a valid postfix expression\n");
 scanf("%s",postfix);


 for(i=0;postfix[i] != '\0'; i++)
 {
   //int isdigit(int c);   return value is nonzero or zero
   if ( isdigit(postfix[i]) )
	st[++top] = postfix[i] - '0';
   else
   {
 	op2 = st[top--];
 	op1 = st[top--];

 	st[++top] = compute(postfix[i],op1,op2);
   }
 }
 printf("Result is %lf\n",st[top--]);
}



Program 3

#include <stdio.h>
#include <stdlib.h>

int* push(int [], int *,int *);
void pop(int [], int *);
void display(int [], int ,int );

int main()  {
    int ch,top=-1; 
    int *s=NULL,n;
    printf("Enter size of stack\n");  
    scanf("%d",&n);
    s = malloc(sizeof(int)*n);
    for(;;)
    {
   	  printf("Stack Simulation\n1: Push\n” 2: Pop\n3: Display\n4: Status\n5. Exit\n");
   	  printf("Enter choice: ");
   	  scanf("%d",&ch);
   	  switch(ch)
   	  {
   		 case 1:  s=push(s,&top,&n);  break;
   		 case 2:  pop(s,&top);   break;
   		 case 3:  display(s,top,n); break;
   		 case 4:  if (n-1 == top)
				     printf("Stack is full\n");
 				  else
               printf("%d Elements can be stored in stack\n",
                                          (top==-1)?n:n-top-1);
                  break;
  	     default: exit(0);
   	  }   
    }  
 }

 void display(int s[],  int top,int n)
 {
    int i;    
    if( top == -1)
   	 printf("stack empty\n");
    else
    {
   	 printf("stack elements are\n");
      printf("TOS is: ");
   	 for(i=top; i>=0; i--)
   	    printf("%d\n",s[i]);
    }
}


int * push(int *s,int *top,int *n) 
{
    int ele;
    if(  (*top)==(*n)-1)
    { 
      printf("stack overflow..doubling the size of stack\n"); 
      *n=(*n)*2;
      s = realloc(s,sizeof(int)*(*n));
    }
    (*top)++;
    printf("enter the element\n"); scanf("%d",&ele);
    s[*top]=ele;
    return s;
}

void pop(int s[],int *top)  
{
    if( (*top) == -1)
   	 printf("stack underflow\n");
    else  
    {
   	 printf("element popped is\n");
   	 printf("%d\n",s[*top]);
   	 (*top)--;
    }     
}

Program 2


#include <stdio.h>
 #include <string.h>
 int slen(char *);
 void replace(char *, char *, char *, char *);
 int main()
 {
 char T[100]; // Main string
 char P[50]; // Pattern to search
 char REP[50]; // Replacement string
 char FIN[200]; // Final string after replacement
 printf("Enter Main string: ");
 gets(T);
 printf("Enter Pattern string: ");
gets(P);
 printf("Enter Replacement string: ");
 gets(REP);
 replace(T, P, REP, FIN);
 printf("Output: %s\n", FIN);
 return 0;
 }
 // Function to calculate string length
 int slen(char *s)
 {
 }
 int len = 0;
 while (s[len] != '\0')
 len++;
 return len;
 // Function to replace all occurrences of P with REP in T and store in FIN
 void replace(char *T, char *P, char *REP, char *FIN)
 {
 int k = 0, s = slen(T), r = slen(P), q = 0, e, z;
 int repLen = slen(REP);
while (k < s)
 {
 }
 // Check if pattern matches at current position
 for (e = 0; e < r; e++)
 {
 }
 if (T[k + e] != P[e])
 break;
 if (e == r) // Pattern found
 {
 }
 for (z = 0; z < repLen; z++)
 FIN[q++] = REP[z];
 k +=r;
 else // No match, copy current character
 {
 }
 FIN[q++] = T[k++];
 FIN[q] = '\0'; // Null terminate final string
 }


 Program 1

 #include <stdio.h>
 #include <string.h>
 struct day
 {
 };
 char name[10];
 int date;
 char activity[50];
 typedef struct day Day;
 void create(Day [], int );
 void display(Day [], int );
 void read(Day [], int );
 int main()
 {
# define SZ 7
 Day planner[SZ];
 create(planner, SZ);
 read(planner, SZ);
 display(planner, SZ);
 return 0;
 }
 void create(Day planner[], int size)
 {
 }
 for (int i = 0; i < size; i++)
 {
 }
 strcpy(planner[i].name, "");
 planner[i].date = 0;
 strcpy(planner[i].activity, "");
 void read(Day planner[], int size)
 {
 for (int i = 0; i < size; i++)
 {
 printf("Day %d:\n", i + 1);
 printf("Enter day name: ");
 scanf("%s", planner[i].name);
 printf("Enter date (numeric): ");
scanf("%d", &planner[i].date);
 printf("Enter activity: ");
 scanf("%s", planner[i].activity);
 }
 }
 void display( Day planner[], int size)
 {
 printf("\n=== Weekly Activity Report ===\n");
 printf("%-10s %-6s %-30s\n", "Day", "Date", "Activity");
 printf("----------------------------------------------\n");
 for (int i = 0; i < size; i++)
 {
 printf("%-10s %-6d %-30s\n", planner[i].name,planner[i].date,
 planner[i].activity);
 }
 }
