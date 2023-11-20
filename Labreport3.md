# Part 1: Bugs  
## This is an input which induces failure  
`@Test
public void testReverseInPlace() {
    int[] input1 = {1, 2, 3 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{3, 2 , 1}, input1);
}`  
## This is an input which dosent induce failure 
`@Test
public void testReverseInPlace() {
    int[] input1 = { 3 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 3 }, input1);
}`
## Output  
input : {1,2,3}  
![image](beforeBugFix.png)  

input : { 3 }  
![image](afterBugFix.png)  

## Bug  
### before  
`static void reverseInPlace(int[] arr) {
  for(int i = 0; i < arr.length; i += 1) {
    arr[i] = arr[arr.length - i - 1];
  }
}`  
### after  
`static void reverseInPlace(int[] arr) {
  int[] newArray = new int[arr.length];
  for(int i = 0;i < arr.length;i++) {
    newArray[i] = arr[i];
  }
  for(int i = 0; i < arr.length; i += 1) {
    arr[i] = newArray[arr.length - i - 1];
} }`  

The first half of values were replaced but the second half was swapped incorrectly as they were suppose to replace the second half but were gone when we put them in the first half 
so they were erased. Now we use a temp array to help us save the values of the old array.

# Part 2  
## grep
I used Chat GPT for this
### -i command 
example 1 
`$ grep -i "study"  biomed/1468-6708-3-1.txt
        events [ 10 ] . In this paper we study whether BMI at
          Study design: The Cardiovascular Health
          Study
          The Cardiovascular Health Study (CHS) is a
          population-based longitudinal study of 5,888 adults aged
        from EVGFP) in the first seven years of the study, adjusted
        CHS Cardiovascular Health Study`
The `-i` command is used to search case-insensitive in the file. This is useful for perfoming searches and patters easily as the issue of case is not there anymore.  
In this case I used "study" as input and it gave me instances where it appeared in lower and upper case.  

example 2  
`$ grep -i "winter"  biomed/1468-6708-3-1.txt biomed/1468-6708-3-1.txt biomed/1468-6708-3-1.txt`
It also works to check patterns in multiple files  
I used it to check how many times winter shows up and it was 0 so there is no output  

### -c command
example 1
`$ grep -c "study"  biomed/1468-6708-3-1.txt
 3`
The `-c` command tells us in how many lines does the pattern occur. This helps us in large files when you want to know how many times is a word used.  
Here I entered 'Study' which occurs 3 times.  

example 2  
`$ grep -c -e "study" -e "bio"  biomed/1468-6708-3-1.txt
3c`  
When we use `-e`, it checks lines which have multiple patterns. It checks the file and checks for patterns in any of the lines. This helps us as we dont have to search a singular line at a time  
Here it checks for study and bio and counts lines which have any of the two patterns.  

### -n command
example 1  
`$ grep -n "study"  biomed/1468-6708-3-1.txt
34: 50: 194:
events [ 10 ] . In this paper we study whether BMI at
  population-based longitudinal study of 5,888 adults aged
 from EVGFP) in the first seven years of the study, adjusted`
 The command `-n` gives us the number of lines which are in a pattern. It helps us find keywords and the words and information around it.  
 When study was used, it gaves us the 3 lines study was used in and the information around it.  

 example 2
 `$ grep -n -c "report" 911report/chapter-1.txt
56`
when we combine `-n` and `-c`, we get the number of times the pattern appeared in a file. We checked report and it gave us 56 cuz the work report appeared 56 times.  

### -r command
example 1  
`$ grep -r "torch"
biomed/ar328.txt:        cryptorchidism.
government/Media/Legal_Aid_attorney.txt:welding torch at sculpting steel.
plos/journal.pbio.0020047.txt:        Creative human beings are the torch-bearers of civilization`
The `-r` command is used to do a recursive search for a pattern in all the files in a dir. It shows instances of occurance in files. This is useful when there a numberous files, and you
 need to find a pattern and context of the occurances.  
I used torch and it gave me the instances it occured in the direectory.  

example 2
`$ grep -r --include="*.txt" "torch"
biomed/ar328.txt:        cryptorchidism.
government/Media/Legal_Aid_attorney.txt:welding torch at sculpting steel.
plos/journal.pbio.0020047.txt:        Creative human beings are the torch-bearers of civilization`
In this case, I used `-r` and on top of it, I used `--include` which checked for the command in only certain files (here we searched in .txt files)

# Part 2 - Acknowledgement of Chat GPT
I used chat GPT in part 2 of the lab report
I asked it the command lines operations for grep and it gave me
`grep is a powerful command-line utility for searching plain-text data sets for lines that match a regular expression. Here are some common command-line operations of grep:
Basic Search:
grep "pattern" file.txt
This command searches for lines in file.txt that contain the specified "pattern."
Case-Insensitive Search:
grep -i "pattern" file.txt
The -i option makes the search case-insensitive.
Search for Whole Words:
grep -w "word" file.txt
The -w option matches whole words, ensuring that "word" is not part of a larger word.
Display Line Numbers:
grep -n "pattern" file.txt
The -n option displays line numbers along with matching lines.
Display Only Matching Part of Lines:
grep -o "pattern" file.txt
The -o option displays only the part of the line that matches the pattern.
Count the Number of Matches:
grep -c "pattern" file.txt
The -c option counts the number of lines that match the pattern.
Recursive Search in Directories:
grep -r "pattern" directory/
The -r (or -R) option performs a recursive search in the specified directory and its subdirectories.
Invert Match (Exclude Lines Containing the Pattern):
grep -v "pattern" file.txt
The -v option inverts the match, showing lines that do not contain the specified pattern.
Search for Multiple Patterns (OR):
grep -e "pattern1" -e "pattern2" file.txt
Use the -e option to specify multiple patterns. It matches lines containing either pattern1 or pattern2.
Pipe Output of Another Command into grep:
command | grep "pattern"
You can use the output of one command as input for grep.
Show Matching Line Before/After/Context:
grep -B 2 "pattern" file.txt   # Display 2 lines before the match
grep -A 2 "pattern" file.txt   # Display 2 lines after the match
grep -C 2 "pattern" file.txt   # Display 2 lines before and after the match
The options -B, -A, and -C control the number of lines to show before, after, or both around the match.`

After that, I used chat gpt to help me combine them with each other and understand other ways to use them together in one grep command.


