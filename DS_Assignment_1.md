# 🧩 Assignment 1: Basic String Operations using Single Dimensional Arrays

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program demonstrates manual implementation of **string operations** — **length**, **copy**, **reverse**, and **concatenation** — using **1-D character arrays** and **dynamic memory allocation**, without using any built-in `<string.h>` functions.

---

## 📝 Problem Statement

Write a C program to perform the following operations manually using **loops**, **character arrays**, and **dynamic memory allocation**:

1. Find **string length**  
2. **Copy** a string  
3. **Reverse** a string  
4. **Concatenate** two strings  

The program must:
- Accept two strings from the user  
- Perform operations without using library functions  
- Use dynamic memory wherever required  
- Display results clearly  
- Free all allocated memory  

---

## 📘 Theory

A **string** in C is a one-dimensional array of characters terminated by the **null character (`'\0'`)**.  
Although C provides predefined string functions, this assignment focuses on **manual implementation**.

---

## 🔹 Key Concepts

### **1. String Length**  
Counts characters until the `'\0'` terminator.

### **2. String Copy**  
Duplicates the string into new memory using `malloc()`.

### **3. String Reverse**  
Copies characters from the end of the string to the beginning.

### **4. String Concatenation**  
Joins two strings into one dynamically allocated array.

---

## 📊 Algorithm

1. Accept two strings: `str1_ssd` and `str2_ssd`.  
2. Calculate their lengths manually.  
3. Copy the first string.  
4. Reverse the first string.  
5. Concatenate both strings.  
6. Display outputs.  
7. Free all dynamically allocated memory.

---

## 💻 CODE

```c
#include <stdio.h>
#include <stdlib.h>

// Function to calculate string length
int stringLength_ssd(char *str_ssd) 
{
    int i_ssd = 0;
    while (str_ssd[i_ssd] != '\0') 
    {
        i_ssd++;
    }
    return i_ssd;
}

// Function to copy string
char* stringCopy_ssd(char *src_ssd) 
{
    int len_ssd = stringLength_ssd(src_ssd);
    char *dest_ssd = (char*)malloc((len_ssd + 1) * sizeof(char));
    for (int i_ssd = 0; i_ssd < len_ssd; i_ssd++) 
    {
        dest_ssd[i_ssd] = src_ssd[i_ssd];
    }
    dest_ssd[len_ssd] = '\0';
    return dest_ssd;
}

// Function to reverse string
char* stringReverse_ssd(char *str_ssd) 
{
    int len_ssd = stringLength_ssd(str_ssd);
    char *rev_ssd = (char*)malloc((len_ssd + 1) * sizeof(char));
    for (int i_ssd = 0; i_ssd < len_ssd; i_ssd++) 
    {
        rev_ssd[i_ssd] = str_ssd[len_ssd - i_ssd - 1];
    }
    rev_ssd[len_ssd] = '\0';
    return rev_ssd;
}

// Function to concatenate two strings
char* stringConcat_ssd(char *str1_ssd, char *str2_ssd) 
{
    int len1_ssd = stringLength_ssd(str1_ssd);
    int len2_ssd = stringLength_ssd(str2_ssd);
    char *result_ssd = (char*)malloc((len1_ssd + len2_ssd + 1) * sizeof(char));

    int i_ssd = 0;
    for (; i_ssd < len1_ssd; i_ssd++) 
    {
        result_ssd[i_ssd] = str1_ssd[i_ssd];
    }
    for (int j_ssd = 0; j_ssd < len2_ssd; j_ssd++) 
    {
        result_ssd[i_ssd++] = str2_ssd[j_ssd];
    }
    result_ssd[i_ssd] = '\0';
    return result_ssd;
}

int main() 
{
    char *str1_ssd = (char*)malloc(100 * sizeof(char));
    char *str2_ssd = (char*)malloc(100 * sizeof(char));

    printf("Enter first string: ");
    scanf("%99s", str1_ssd);

    printf("Enter second string: ");
    scanf("%99s", str2_ssd);

    // Length
    printf("\nLength of first string: %d\n", stringLength_ssd(str1_ssd));
    printf("Length of second string: %d\n", stringLength_ssd(str2_ssd));

    // Copy
    char *copy_ssd = stringCopy_ssd(str1_ssd);
    printf("Copy of first string: %s\n", copy_ssd);

    // Reverse
    char *rev_ssd = stringReverse_ssd(str1_ssd);
    printf("Reverse of first string: %s\n", rev_ssd);

    // Concatenation
    char *concat_ssd = stringConcat_ssd(str1_ssd, str2_ssd);
    printf("Concatenation of both strings: %s\n", concat_ssd);

    // Free memory
    free(str1_ssd);
    free(str2_ssd);
    free(copy_ssd);
    free(rev_ssd);
    free(concat_ssd);

    return 0;
}
```

---

## 🧮 Example Execution

**Input**
```
Saee
Darwatkar
```

**Output**
```
Length of first string: 4
Length of second string: 9
Copy of first string: Saee
Reverse of first string: eeaS
Concatenation of both strings: SaeeDarwatkar
```

---

## 🧠 Memory Visualization

| Variable       | Description              | Representation                                |
|----------------|--------------------------|-----------------------------------------------|
| `str1_ssd`     | First input string       | [S][a][e][e][\0]                               |
| `str2_ssd`     | Second input string      | [D][a][r][w][a][t][k][a][r][\0]                |
| `copy_ssd`     | Copy of `str1_ssd`       | [S][a][e][e][\0]                               |
| `rev_ssd`      | Reversed string          | [e][e][a][S][\0]                               |
| `concat_ssd`   | Combined string          | [S][a][e][e][D][a][r][w][a][t][k][a][r][\0]    |

---

## ✅ Conclusion

This assignment demonstrates how basic string operations can be implemented manually using **loops**, **character arrays**, and **dynamic memory allocation**, without relying on built-in library functions.  
It strengthens understanding of:
- Pointer manipulation  
- Memory management (`malloc`, `free`)  
- Manual string processing  