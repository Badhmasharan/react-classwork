
1. frontend:
#include <stdio.h>
#include <ctype.h>
#include <string.h>

int ag = 0, z = 1;

void main() {
    char a[50], id[50], b[50], op[50];
    char mov[] = "MOVF", mul[] = "MULF", div[] = "DIVF", add[] = "ADDF", sub[] = "SUBF";
    int ti = 0;
    int i = 0, j = 0, k = 0, len = 0, s = 0, e = 0, r = 1, count;
    FILE *fp;

    fp = fopen("out.txt", "w");
    printf("\nEnter the code: ");
    scanf("%s", a);
    strcpy(b, a);
    len = strlen(a);

    for (i = 0; i < strlen(b); i++) {
        if (b[i] == '*' || b[i] == '/') {
            for (j = i - 1; b[j] != '-' && b[j] != '+' && b[j] != '*' && b[j] != '/' && b[j] != '='; j--);
            k = j + 1;
            count = 0;

            printf("\nt%d=", ti++);

            for (j = j + 1; count < 2 && b[j] != '\0'; j++) {
                if (b[j + 1] == '+' || b[j + 1] == '-' || b[j + 1] == '*' || b[j + 1] == '/')
                    count++;
                printf("%c", b[j]);
            }

            b[k++] = 't';
            b[k++] = ti - 1 + 48;

            for (j = j, k = k; k < strlen(b); k++, j++)
                b[k] = b[j];

            i = 0;
        }
    }

    for (i = 0; i < strlen(b); i++) {
        if (b[i] == '+' || b[i] == '-') {
            for (j = i - 1; b[j] != '-' && b[j] != '+' && b[j] != '='; j--);
            k = j + 1;
            count = 0;

            printf("\nt%d=", ti++);

            for (j = j + 1; count < 2 && b[j] != '\0'; j++) {
                if (b[j + 1] == '+' || b[j + 1] == '-')
                    count++;
                printf("%c", b[j]);
            }

            b[k++] = 't';
            b[k++] = ti - 1 + 48;

            for (j = j, k = k; k < strlen(b); k++, j++)
                b[k] = b[j];
        }
    }

    printf("\n%s", b);
}

2.backend:
#include<stdio.h>
#include<ctype.h>
#include<string.h>
int ag=0,z=1;
void main()
{
	char
	a[50],id[50],mov[]="MOVF",mul[]="MULF",div[]="DIVF",add[]="ADDF",sub[]="SUBF";
	int i=0,j=0,len=0,s=0,e=0,r=1;
	FILE *fp;
	fp=fopen("out.txt","w");
	printf("\nEnter the code:");
	gets(a);
	len=strlen(a);
	for(i=0; i<len; i++)
	{
		if(a[i]=='=')
		{
			for(j=i; j<len; j++)
				if(a[j]=='i')
				{
					fprintf(fp,"\n%s ",mov);
					fprintf(fp,"%c%c%c,R%d",a[j],a[j+1],a[j+2],r++);
				}
		}
		else if((a[i]<=57)&&(a[i]>=48))
			if((a[i+1]<=57)&&(a[i+1]>=48))
				fprintf(fp,"\n%s #%c%c,R%d",mov,a[i],a[i+1],r++);
	}
	for(i=len-1; i>=0; i--)
	{
		if(a[i]=='+')
		{
			fprintf(fp,"\n%s ",add);
			e=a[i-1];
			e--;
			s=e;
			if(a[i+1]=='i')
				fprintf(fp,"R%c,R%d",e,r-1);
		}
		else if(a[i]=='-')
		{
			fprintf(fp,"\n%s ",sub);
			e=a[i-1];
			e--;
			s=e;
			if(a[i+1]=='i')
				fprintf(fp,"R%c,R%c",(a[i+3]-1),s);
			else
				fprintf(fp,"R%c,R%d",e,r-1);
		}
		else if(a[i]=='*')
		{
			fprintf(fp,"\n%s ",mul);
			e=a[i-1];
			e--;
			s=e;
			if(a[i+1]=='i')
				fprintf(fp,"R%c,R%c",(a[i+3]-1),s);
			else
				fprintf(fp,"R%c,R%d",e,r-1);
		}
		else if(a[i]=='/')
		{
			fprintf(fp,"\n%s ",div);
			e=a[i-1];
			e--;
			s=e;
			if(a[i+1]=='i')
				fprintf(fp,"R%c,R%c",(a[i+3]-1),s);
			else
				fprintf(fp,"R%c,R%d",e,r-1);
		}
	}
	fprintf(fp,"\n%s R1,id1",mov);
}

3. c lexical:
#include <stdio.h>
#include <string.h>
#include <ctype.h>

void keyword(char str[10]) {
    char keywords[10][10] = {"int", "float", "char", "while", "do", "for", "if"};
    int isKeyword = 0;
    for (int i = 0; i < 7; i++) {
        if (strcmp(keywords[i], str) == 0) {
            isKeyword = 1;
            break;
        }
    }
    if (isKeyword)
        printf("\n%s is a keyword", str);
    else
        printf("\n%s is an identifier", str);
}

int main() {
    FILE *f1, *f2, *f3, *f4;
    char c, str[10];
    int num[100], tokenvalue = 0, i = 0, j = 0, k = 0;

    printf("\nEnter the C program (End with Ctrl+D for EOF):\n");
    f1 = fopen("input.txt", "w");
    while ((c = getchar()) != EOF)
        putc(c, f1);
    fclose(f1);

    f1 = fopen("input.txt", "r");
    f2 = fopen("identifier.txt", "w");
    f3 = fopen("specialchar.txt", "w");
    f4 = fopen("operators.txt", "w");

    while ((c = getc(f1)) != EOF) {
        if (isdigit(c)) {
            tokenvalue = c - '0';
            c = getc(f1);
            while (isdigit(c)) {
                tokenvalue = tokenvalue * 10 + (c - '0');
                c = getc(f1);
            }
            num[i++] = tokenvalue;
            ungetc(c, f1);
        } else if (isalpha(c)) {
            putc(c, f2);
            c = getc(f1);
            while (isalnum(c) || c == '_' || c == '$') {
                putc(c, f2);
                c = getc(f1);
            }
            putc(' ', f2);
            ungetc(c, f1);
        } else if (c == '+' || c == '-' || c == '*' || c == '<' || c == '>' || c == '/' || c == '&' || c == '%' || c == '^' || c == '=') {
            putc(c, f4);
        } else {
            putc(c, f3);
        }
    }

    fclose(f1);
    fclose(f2);
    fclose(f3);
    fclose(f4);

    printf("\nThe constants are: ");
    for (j = 0; j < i; j++) {
        printf("%d ", num[j]);
    }
    printf("\n");

    f2 = fopen("identifier.txt", "r");
    printf("The keywords and identifiers are:");
    while ((c = getc(f2)) != EOF) {
        if (c != ' ') {
            str[k++] = c;
        } else {
            str[k] = '\0';
            keyword(str);
            k = 0;
        }
    }
    fclose(f2);

    f3 = fopen("specialchar.txt", "r");
    printf("\nSpecial characters are: ");
    while ((c = getc(f3)) != EOF) {
        printf("%c ", c);
    }
    fclose(f3);

    f4 = fopen("operators.txt", "r");
    printf("\nOperators are: ");
    while ((c = getc(f4)) != EOF) {
        printf("%c ", c);
    }
    printf("\n");
    fclose(f4);

    return 0;
}


4. optimized 10th:
#include<stdio.h>
#include<string.h>
struct op
{
	char l;
	char r[20];
} op[10],pr[10];
void main()
{
	int a,i,k,j,n,z=0,m,q;
	char *p,*l,*tem,temp,t;
	char nu[]="\0";
	printf("\nEnter the no of values:");
	scanf("%d",&n);
	for(i=0; i<n; i++)
	{
		printf("\nLeft ");
		scanf("%s",&op[i].l);
		printf("Right ");
		scanf("%s",op[i].r);
	}
	printf("\nIntermediate code\n");
	for(i=0; i<n; i++)
		printf("%c=%s\n",op[i].l,op[i].r);
	for(i=0; i<n; i++)
	{
		temp=op[i].l;
		p=NULL;
		for(j=0; j<n; j++)
		{
			p=strchr(op[j].r,temp);
			if(p)
			{
				pr[z].l=op[i].l;
				strcpy(pr[z].r,op[i].r);
				z++;
				break;
			}
		}
	}
	printf("\nAfter dead code elimination\n");
	for(k=0; k<z; k++)
		printf("%c\t=%s\n",pr[k].l,pr[k].r);
	for(m=0; m<z; m++)
	{
		tem=pr[m].r;
		for(j=m+1; j<z; j++)
		{
			p=strstr(tem,pr[j].r);
			if(p)
			{
				pr[j].l=pr[m].l;
				for(i=0; i<z; i++)
				{
					if(l)
					{
						a=l-pr[i].r;
						pr[i].r[a]=pr[m].l;
					}
				}
			}
		}
	}
	printf("\nEliminate common expression\n");
	for(i=0; i<z; i++)
		printf("%c\t=%s\n",pr[i].l,pr[i].r);
	for(i=0; i<z; i++)
	{
		for(j=i+1; j<z; j++)
		{
			q=strcmp(pr[i].r,pr[j].r);
			if((pr[i].l==pr[j].l)&&!q)
			{
				pr[i].l='\0';
				strcpy(pr[i].r,nu);
			}
		}
	}
	printf("\nOptimized code\n");
	for(i=0; i<z; i++)
		if(pr[i].l!='\0')
			printf("%c\t=%s\n",pr[i].l,pr[i].r);
}

5. symbol table:

#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <ctype.h>

#define MAX_SYMBOLS 100

struct SymbolTableEntry {
    char variable[50];
    char datatype[10];
    int line_no;
    char value[50];
};

struct SymbolTableEntry symbolTable[MAX_SYMBOLS];
int symbolCount = 0;


void addSymbol(char* datatype, char* variable, int line_no, char* value) {
    strcpy(symbolTable[symbolCount].datatype, datatype);
    strcpy(symbolTable[symbolCount].variable, variable);
    symbolTable[symbolCount].line_no = line_no;
    strcpy(symbolTable[symbolCount].value, value);
    symbolCount++;
}


void printSymbolTable() {
    printf("\nSYMBOL TABLE MANAGEMENT\n");
    printf("Variable\tDatatype\tLine.no\tValue\n");
    for (int i = 0; i < symbolCount; i++) {
        printf("%s\t\t%s\t\t%d\t%s\n", symbolTable[i].variable,
               symbolTable[i].datatype, symbolTable[i].line_no, symbolTable[i].value);
    }
}

int main() {
    int n, line_no = 1;
    char datatype[10], variable[50], value[50];

    printf("Enter the number of variable declarations: ");
    scanf("%d", &n);

    for (int i = 0; i < n; i++) {
        printf("Enter datatype, variable name ");
        scanf("%s %s", datatype, variable);

       
        char *equalSign = strchr(variable, '=');
        if (equalSign != NULL) {
            
            *equalSign = '\0'; 
            strcpy(value, equalSign + 1); 
        } else {
            strcpy(value, "garbage"); 
        }

       
        char *semicolon = strchr(variable, ';');
        if (semicolon != NULL) {
            *semicolon = '\0';
        }

        
        addSymbol(datatype, variable, line_no++, value);
    }

    printSymbolTable();

    return 0;
}
