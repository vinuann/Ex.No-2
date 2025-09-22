# Ex.No :2
# GENERATION OF LEXICAL TOKENS USING LEX/FLEX TOOL
## Name: Vinuthaa NN
## Register Number: 212224040362
## Date:22-09-2025
## AIM
 To write a lex program to implement lexical analyzer to recognize a few patterns.
## ALGORITHM

1.	Start the program.

2.	Lex program consists of three parts.

     a.	Declaration %%

     b.	Translation rules %%

     c.	Auxilary procedure.

3.	The declaration section includes declaration of variables, maintest, constants and regular definitions.
4.	Translation rule of lex program are statements of the form

    a.	P1 {action}

    b.	P2 {action}

    c.	…

    d.	…

    e.	Pn {action}

5.	Write a program in the vi editor and save it with .l extension.

6.	Compile the lex program with lex compiler to produce output file as lex.yy.c. eg $ lex filename.l $ cc lex.yy.c
7.	Compile that file with C compiler and verify the output.

## PROGRAM:
```
%{
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int keywordsCount = 0, identifiersCount = 0, numbersCount = 0, operatorsCount = 0, othersCount = 0;

char *keywords[] = {
    "auto","break","case","char","const","continue","default","do","double",
    "else","enum","extern","float","for","goto","if","int","long","register",
    "return","short","signed","sizeof","static","struct","switch","typedef",
    "union","unsigned","void","volatile","while"
};

int isKeyword(char *word) {
    int i;   /* declare outside the loop for C89 */
    for(i = 0; i < 32; i++) {
        if(strcmp(keywords[i], word) == 0)
            return 1;
    }
    return 0;
}
%}

%%
"/*"([^*]|\*+[^*/])*\*+"/"    { printf("\n COMMENT : %s", yytext); }
"//".*                        { printf("\n COMMENT : %s", yytext); }

[ \t\n]+                      { /* skip whitespace */ }

"int"|"float"|"char"|"double"|"long"|"short"|"void" {
                                printf("\n KEYWORD : %s", yytext); keywordsCount++; }

[0-9]+                        { printf("\n NUMBER : %s", yytext); numbersCount++; }

[a-zA-Z_][a-zA-Z0-9_]*        {
                                if(isKeyword(yytext)) {
                                    printf("\n KEYWORD : %s", yytext); keywordsCount++;
                                } else {
                                    printf("\n IDENTIFIER : %s", yytext); identifiersCount++;
                                }
                              }

"="|"+"|"-"|"*"|"/"           { printf("\n OPERATOR : %s", yytext); operatorsCount++; }

"<="|">="|"=="|"<"|">"        { printf("\n RELATIONAL OPERATOR : %s", yytext); operatorsCount++; }

";"|","|"(" | ")" | "{" | "}" { printf("\n SPECIAL SYMBOL : %s", yytext); othersCount++; }

.                             { printf("\n UNKNOWN TOKEN : %s", yytext); othersCount++; }
%%

int main(int argc, char *argv[]) {
    if(argc < 2) {
        printf("Usage: %s <sourcefile>\n", argv[0]);
        return 1;
    }
    FILE *fp = fopen(argv[1], "r");
    if(!fp) {
        perror("File open failed");
        return 1;
    }
    yyin = fp;
    yylex();
    printf("\n\n--- SUMMARY ---");
    printf("\n Keywords   : %d", keywordsCount);
    printf("\n Identifiers: %d", identifiersCount);
    printf("\n Numbers    : %d", numbersCount);
    printf("\n Operators  : %d", operatorsCount);
    printf("\n Others     : %d\n", othersCount);
    fclose(fp);
    return 0;
}

int yywrap() {
    return 1;
}
```

## INPUT:
```
int main() {
    int x;
}
```
## OUTPUT:

<img width="672" height="849" alt="image" src="https://github.com/user-attachments/assets/c478e266-3d01-40b5-ac91-4f5fafee88c5" />

## RESULT:
 The lexical analyzer is implemented using lex and the output is verified.
