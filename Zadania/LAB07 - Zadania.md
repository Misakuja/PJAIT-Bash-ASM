1. Napisz program, który odczyta i wydrukuje na terminalu znak po znaku zawartość pliku. Każdy znak powiększ przed drukiem o 1, aby plik tekstowy z ciągiem 10asdf wydrukował ciąg ‘21bteg’
```C
#include <stdio.h>  
#include <stdlib.h>  
#include <unistd.h>  
  
int main(int argc, char *argv[]) {  
    char fileContentToWrite[6] = "10asdf";  
  
    FILE *file = fopen("LAB07z1.txt", "w+");  
    if (file == NULL) {  
       printf("Error opening file\n");  
       return 1;  
    }  
  
    fprintf(file, "%s", fileContentToWrite);  
  
    rewind(file);  
  
    int character;  
    while ((character = fgetc(file)) != EOF) {  
       character++;  
       printf("%c", character);  
    }  
  
    fclose(file);  
  
    return 0;  
}
```

2. Przerób poprzedni program (polecam go skopiować i pracować na nowoutworzonym) tak, aby zapisywał do nowego pliku to, co w poprzednim programie drukował.
```C
#include <stdio.h>  
#include <stdlib.h>  
#include <unistd.h>  
  
void checkIfFileOpenedFine(const FILE *file) {  
    if (file == NULL) {  
       printf("Error opening file\n");  
       exit(1);  
    }  
}  
  
int main(int argc, char *argv[]) {  
    char fileContentWrite[6] = "10asdf";  
  
    FILE *file = fopen("LAB07z2.txt", "w");  
    checkIfFileOpenedFine(file);  
  
    fprintf(file, "%s", fileContentWrite);  
    fclose(file);  
  
    file = fopen("LAB07z2.txt", "r");  
    checkIfFileOpenedFine(file);  
  
    FILE *outputFile = fopen("output.txt", "w");  
    checkIfFileOpenedFine(file);  
  
    int character;  
    while ((character = fgetc(file)) != EOF) {  
       character++;  
       fprintf(outputFile, "%c", character);  
    }  
    fclose(file);  
    fclose(outputFile);  
    return 0;  
}
```

3. Napisz program, który zapisze wprowadzony przez użytkownika tekst do pliku tekstowego. Następnie program powinien wczytać zapisany tekst i wyświetlić go na ekranie.
```C
#include <stdio.h>  
#include <stdlib.h>  
#include <unistd.h>  
  
void checkIfFileOpenedFine(const FILE *file) {  
    if (file == NULL) {  
       printf("Error opening file\n");  
       exit(1);  
    }  
}  
  
int main(int argc, char *argv[]) {  
    char input[50];  
    char output[50];  
  
    scanf("%s", input);  
    FILE *file = fopen("LAB07z3.txt", "w");  
    checkIfFileOpenedFine(file);  
  
    fprintf(file, "%s", input);  
  
    fclose(file);  
  
    file = fopen("LAB07z3.txt", "r");  
    checkIfFileOpenedFine(file);  
  
    while (fscanf(file, "%s", output) != EOF) {  
       printf("%s ", output);  
    }  
    fclose(file);  
  
    return 0;  
}
```

4. Napisz program, który odczyta dane z pliku tekstowego zawierającego liczby całkowite, a następnie policzy ich sumę oraz średnią arytmetyczną. Program powinien wyświetlić na ekranie wyniki obliczeń.
```C
#include <stdio.h>  
#include <stdlib.h>  
#include <unistd.h>  
  
void checkIfFileOpenedFine(const FILE *file) {  
    if (file == NULL) {  
       printf("Error opening file\n");  
       exit(1);  
    }  
}  
  
int main(int argc, char *argv[]) {  
    char fileContentWrite[14] = "10 20 30 40 50";  
  
    FILE *file = fopen("LAB07z4.txt", "w+");  
    checkIfFileOpenedFine(file);  
  
    fprintf(file, "%s", fileContentWrite);  
  
    rewind(file);  
  
    int number, sum = 0, counter = 0;  
    while (fscanf(file, "%d", &number) != EOF) {  
       sum += number;  
       counter++;  
    }  
  
    const int avg = sum / counter;  
    printf("The sum is: %d\n", sum);  
    printf("Average is %d\n", avg);  
  
    fclose(file);  
    return 0;  
}
```

5. Napisz program, który utworzy nowy plik tekstowy i zapisze do niego listę pięciu imion i nazwisk, wczytanych z klawiatury przez użytkownika. Następnie program powinien posortować alfabetycznie listę i wyświetlić ją na ekranie.
```C
#include <stdio.h>  
#include <stdlib.h>  
#include <unistd.h>  
#include <ctype.h>  
  
void checkIfFileOpenedFine(const FILE *file) {  
    if (file == NULL) {  
       printf("Error opening file\n");  
       exit(1);  
    }  
}  
  
int alphabetize(const char *a, const char *b) {  
    while (*a && *b) { //continue until one string ends  
       if (tolower(*a) != tolower(*b)) return tolower(*a) - tolower(*b); //compare ascii  
       a++, b++; //move to next char in string (char arr)  
    }  
    return tolower(*a) - tolower(*b);  
}  
  
void swapStrings(char *a, char *b) {  
    for (int i = 0; i < 50; ++i) {  
       const char temp = a[i];  
       a[i] = b[i];  
       b[i] = temp;  
       if (a[i] == '\0' && b[i] == '\0') break; //continue until one string ends  
    }  
}  
  
int main(int argc, char *argv[]) {  
    FILE *file = fopen("LAB07z5.txt", "w+");  
    checkIfFileOpenedFine(file);  
  
    char names[5][50];  
  
    for (int i = 0; i < 5; ++i) {  
       fgets(names[i], 49, stdin);  
        fprintf(file, "%s", names[i]);  
    }  
  
    rewind(file);  
  
    for (int i = 0; i < 5; i++) {  
       fgets(names[i], 49, file);  
    }  
  
    for (int i = 0; i < 4; i++) {  
       for (int j = i + 1; j < 5; j++) {  
          if (alphabetize(names[i], names[j]) > 0) {  
             swapStrings(names[i], names[j]);  
          }  
       }  
    }  
  
    for (int i = 0; i < 5; i++) {  
       printf("%s", names[i]);  
    }  
  
    fclose(file);  
    return 0;  
}
```