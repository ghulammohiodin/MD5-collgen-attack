# MD5-collgen-attack


# INSTRUCTIONAAL PDF 
   ![pdf link]https://seedsecuritylabs.org/Labs_20.04/Files/Crypto_MD5_Collision/Crypto_MD5_Collision.pdf
this is the md5 collgen attack lab 
# Question 1: If the length of your prefix file is not a multiple of 64, what is going to happen?
     Answer: It will be padded with zeros

# Question 2: Create a prefix file with exactly 64 bytes, and run the collision tool again, and see what happens.
     Answer: No zero padding is observed

# Question 3. Are the data (128 bytes) generated by md5collgen completely different for the two output files? Please identify all the bytes that are different.
    Answer: No, not all bytes are different. In previous case, the bytes only differ at the positions 20, 60, 84 and 110. After multiple trials, it is noted that these differences are not constant.
# Task 1  Generating Two Different Files with the Same MD5 Hash
 #make file 
 
 
           $ touch prefix.txt
# Create prefix #
     echo "md5collgen attack lab" >> prefix.txt

# Generate MD5 Collision #
     md5collgen -p prefix.txt -o out1.bin out2.bin

# Compare binary files 
     diff out1.bin out2.bin
![task1](https://github.com/ghulammohiodin/MD5-collgen-attack/blob/381c132e1718d7f04ca803a685981588819f62b5/task1_1.png)
# Check MD5 Hashes #
     md5sum out1.bin ; md5sum out2.bin

# Understand bin file contents #
    xxd out1.bin
    
![task1](https://github.com/ghulammohiodin/MD5-collgen-attack/blob/381c132e1718d7f04ca803a685981588819f62b5/task1.png)

# Compare difference when prefix is mult. of 64 #
    echo "$(python -c 'print("A"*63)')" >> prefix_64.txt
    echo "Generating MD5 Collision...\n"
    md5collgen -p prefix_64.txt -o out1_64.bin out2_64.bin
    xxd out1.bin ; xxd out1_64.bin
# Task2 Understanding MD5’s Property

   # Generate prefix and suffix that is mult of 64 #
     echo "$(python -c 'print("A"*63)')" > prefix

# Create collision 
     md5collgen -p prefix -o out1.bin out2.bin

# Append suffix #cancatinate suffix
     cat out1.bin suffix > file1
     cat out2.bin suffix > file2

# Are checksums the same? #
    md5sum file1 ; md5sum file2
![task2](https://github.com/ghulammohiodin/MD5-collgen-attack/blob/381c132e1718d7f04ca803a685981588819f62b5/task2.png)


# Task 3  Generating Two Executable Files with the Same MD5 Has
# Fill xyz array #
     echo "$(python -c 'print("A"*199)')"
# OR

     echo "$(python -c 'print("0x41,"*199)')"

# Compile C Program 
     gcc task3.c -o task3.o
     
     #include <stdio.h>
     unsigned char xyz[200] = {
    /* The actual contents of this array are up to you */
     };
     int main()
     {
    int i;
        for (i=0; i<200; i++){
               printf("%x", xyz[i]);
     }
     printf("\n");
      }


# Find offset of xyz array using bless 
    bless task3.o
![task3 bless](https://github.com/ghulammohiodin/MD5-collgen-attack/blob/381c132e1718d7f04ca803a685981588819f62b5/task3_1.png)
# Create prefix  
    head -c 4160 task3.o >> prefix

# Generate P and Q values 
     md5collgen -p prefix -o out_p.bin out_q.bin

# Isolate P and Q values 
      tail -c 128 out_p.bin > p
     tail -c 128 out_q.bin > q

# Find offset for suffix 
     tail -c +4288 task3.o > suffix

# Build executable 
     cat prefix p suffix > task3_1
     chmod +x task3_1
 ![task3](https://github.com/ghulammohiodin/MD5-collgen-attack/blob/381c132e1718d7f04ca803a685981588819f62b5/task3_2.png)
 
# Task 4  Making the Two Programs Behave Differently

 # C program for task 4
          #include <stdio.h>

     unsigned char x[200] = {
     0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,

        };

    unsigned char y[200] = {
     0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,0x41,

     };

    int main() {
     int i;
     for (i=0; i<200; i++){
    if(x[i] != y[i]){ break; }
    }

     if(i == 200){ printf("%s", "benign code"); } /* x = y */
     else{ printf("%s", "WARNING: malicious code"); } /* x != y */

     printf("\n");
     }

     tail -c +4289 task4.o > suffix
   # Benign file: prefix | p | suffix1 | p | suffix2
     head -c 96 suffix > suffix_1
    tail -c +225 suffix > suffix_2
    tail -c 3360 suffix > suffix_2
   ![task4 OUTPUT](https://github.com/ghulammohiodin/MD5-collgen-attack/blob/381c132e1718d7f04ca803a685981588819f62b5/task4_1.png)
 
