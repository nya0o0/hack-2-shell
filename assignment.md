## Quesion 1: get the data file
Download the following data files from the internet using the `curl` command: http://eaton-lab.org/pdsb/test.fastq.gz and http://eaton-lab.org/pdsb/iris-data-dirty.csv. Use the `less` or `zless` commands to look at each file. Describe what these commands do. Finally, use the `head` command to print the first 5 lines of the file `iris-data-dirty.csv`.

First, I ued man curl to find the using method of the `curl` command. I found that a `-O` option is needed to save the file with the same name as in the URL. Therefore, I used `curl` command with the `-O` option to download the two files. Then, I use `ls` command to confirm that the file has been saved.

Second, I read the man page of the `zless` and `less` commands. The `less` command is used to view the contents of a file. It does not have to read the entire input file before  starting, so I may start faster than text editors when handling large files. Compared to the `less` command, the `zless` command can be used to view compressed or plain text files. Therefore, I decided to use `less` to view the `iris-data-dirty.csv` file and use `zless` to view the `test.fastq.gz file`.

Third, I read the man page of the `head` command. I found that I need to use the `-n 5` description to set it to read the first 5 lines of the file.

Solution:
```
# Download the files from the URL
curl -O http://eaton-lab.org/pdsb/test.fastq.gz 
curl -O http://eaton-lab.org/pdsb/iris-data-dirty.csv

# Test whether the files are downloaded successfully
ls
```
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
```
<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx</center>
```
