## Quesion 1: get the data file
Download the following data files from the internet using the `curl` command: http://eaton-lab.org/pdsb/test.fastq.gz and http://eaton-lab.org/pdsb/iris-data-dirty.csv. Use the `less` or `zless` commands to look at each file. Describe what these commands do. Finally, use the `head` command to print the first 5 lines of the file `iris-data-dirty.csv`.

First, I used `man curl` to find the using method of the `curl` command. I found that a `-O` option is needed to save the file with the same name as in the URL. Therefore, I used `curl` command with the `-O` option to download the two files. Then, I use `ls` command to confirm that the file has been saved.

Second, I read the man page of the `zless` and `less` commands. The `less` command is used to view the contents of a file. It does not have to read the entire input file before starting, so I may start faster than text editors when handling large files. Compared to the `less` command, the `zless` command can be used to view compressed or plain text files. Therefore, I decided to use `less` to view the `iris-data-dirty.csv` file and use `zless` to view the `test.fastq.gz file`. However, when I viewed the file content, It showed a error message `301 Moved Permanently`. I read the mannule of `curl` command again and a -L option needed for the sitution that the server reports that the requested page has moved to a different location. Therefore, I re-downloaded the files with `curl -L -O` command and view them again, this time the files looked correct.

Third, I read the man page of the `head` command. I found that I need to use the `-n 5` description to set it to read the first 5 lines of the file. 

Solution:
```
# Download the files from the URL
curl -O http://eaton-lab.org/pdsb/test.fastq.gz 
curl -O http://eaton-lab.org/pdsb/iris-data-dirty.csv

# Test whether the files are downloaded successfully
ls
```
Output 1:
```
abc.txt  file.txt  iris-data-dirty.csv  test.fastq.gz
```


```
# View the contents of the files
less iris-data-dirty.csv
zless test.fastq.gz

# Print the first 5 lines of iris-data-dirty.csv
head -n 5 iris-data-dirty.csv
```
Output 2:
```
5.1,3.5,1.4,0.2,Iris-setosa
4.9,3.0,1.4,0.2,Iris-setosa
4.7,3.2,1.3,0.2,Iris-setosa
4.6,3.1,1.5,0.2,Iris-setosa
5.0,3.6,1.4,0.2,Iris-setosa
```

## Quesion 2: clean the data
Use `grep`, `uniq`, and `sed` for this question. Check that all of the species names are spelled correctly in the file `iris-data-dirty.csv`. Also check for missing values stored as NA. Create a new file where mispelled names are replaced with the correct values, and lines with NA are excluded, and save it as `iris-data-clean.csv`. Use `cut`, `sort` and `uniq` to list the number of data values there are for each species in the new cleaned data file. Describe your work.

First, I used `man` to see the manual of of the `grep`, `uniq`, and `sed` commands. The `grep` command can print the lines with matched, the `uniq` command can omit repeated lines and `sed` can edit text in a input stream. Therefore, I tried to use `grep` to get the fifth colum of the data, and then use `uniq` to omit the repeated lines. However, I found that the output has repeated `Iris-setosa` lines. After that, I looked for command to solve this and found I can use `sort` before using `uniq`. 

Second, to edit the mispelled names and delete the lines with `NA`, I used `sed` and `grep -v` and write the cleaned data to a new file ` iris-data-clean.csv`.

Third, I read the man page of the `cut` command. It can remove sections from each line of files, so it can be used to get the last column of the cleaned file. According to the mannual, in the `cut` command, `-d` option can change the field delimiter, and `-f` can select the given field, so they are used to get the fifth column of the data. Then, similar to step 1, I used `sort` and `uniq` to delete the repeated lines, but add a `-c` option to count the number of data values there are for each species. However, I found that there is a line that count 1 for an empty species name in the output, therefore, I used `grep -v` again to delete the empty line.

Solution:
```
grep -o "[^,]*$" iris-data-dirty.csv | sort | uniq
```
Output 1:
```
Iris-setosa
Iris-setsa
Iris-versicolor
Iris-versicolour
Iris-virginica
```

```
sed 's/Iris-setsa/Iris-setosa/; s/Iris-versicolour/Iris-versicolor/' iris-data-dirty.csv | grep -v "NA" | grep -v '^$' > iris-data-clean.csv

cut -d ',' -f 5 iris-data-clean.csv | sort | uniq -c
```
Output 2:
```
     50 Iris-setosa
     48 Iris-versicolor
     50 Iris-virginica
```



## Quesion 3: find a sequence
Find how many lines in the data file ·test.fastq.gz` start with "TGCAG" and end with "GAG". Describe your work.

First, I searched for what command can be used to search in a compressed file. Then, I found that `zgrep` can do this and found the mannual of it. The manual toled me to use the mannual of `grep` for more detialed operations. In the `grep` mannual, I found that `-c` option is required for counting the number of matched lines, and the  caret ` ^ ` and  the  dollar sign `$` are meta-characters that respectively match the empty string at the beginning and end of a line. Also, `.` matches any single character (except a newline), and `*` matches zero or more occurrences of the preceding element. Together, `.*` means "match zero or more of any character". Therefore, using this information, I found the answer is 44.

Solution:

```
zgrep -c "^TGCAG.*GAG$" test.fastq.gz
```
Output
```
44
```

## Quesion 3: Find a sequence chunk
Using `grep` and other tools if necessary find all lines that contain the sequence "AAAACCCC" and for each print that line, the line above it, and two lines below it (so that a 4-line chunk around each search hit is printed). Describe your work.

In the mannual of `grep`, `-A` is used to print given number of lines after the matching lines, and `-B` is used to print given number of lines before the matching lines. Also, `zgrep` is needed for compressed file. Therefore, I used `zgrep -A -B` to do this.

Solution:
```
zgrep -B 1 -A 2 "AAAACCCC" test.fastq.gz
```
Output:
```
@32082_przewalskii.98 GRC13_0027_FC:4:1:5669:1669 length=74
TGCAGAATAGATAGGAAACGTTTTGGCGCTGTAGACATTAAAACCCCAGTAGGACACGGGTATCACAACGTACA
+32082_przewalskii.98 GRC13_0027_FC:4:1:5669:1669 length=74
IIIIIIIIIIIIIIIIIIHIHIIIIIIIIGIIIGIIIIIIHIIIIIIIHIIIIHIIIIIIEHIHHIIIIICIHI
--
@33413_thamno.59 GRC13_0027_FC:4:1:5000:1620 length=74
TGCAGTGGATCGAAAACCCCGAGGCTCAAGGTCACGCCACCGTCTTCGTGGCCAAGTTCTTCGGCCGCGCCGGC
+33413_thamno.59 GRC13_0027_FC:4:1:5000:1620 length=74
IIIIIIIIIIIIIIIIIIIIDHIIHHIIIIIEIBGBGGGIIHEHHHIEBBHHIEGGDGIGGHAEFDBFBDDB?D
```
