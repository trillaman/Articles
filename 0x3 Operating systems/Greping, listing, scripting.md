# Greping, listing, scripting

## Listing files

To list files from example folder like /etc where there is one letter starting file/catalog name and second is "a" you can find like this

```ls -la /etc/?a*```  

If for example in whole name have to be "m" and then "l" wherever in name you can use this  

```ls -la /etc/*m*l*```  

To list only files (no catalogs), and put output to file you can use this  

```ls /etc -p | grep -v / > output.txt```  
grep -v / is here to omit names that ends with slash (catalogs)  
switch -p is here to add slashes to catalogs in listing to easier separate them with grep  


## Greping

This one will find lines in file that contains am OR al OR ma  
```grep -E 'am|al|ma' file.txt```

This one will find strings that starts with am, lm, aa or la and have other letters after that.
-o stands to show only matching  
```grep -o "^am\S*\|^lm\S*\|^aa\S*\|^la\S*"```


## Scripting

### Script 1 - variables from command line  
Below script will take all arguments passed to it and do some math expressions  
```
#!/bin/bash

var1=6
var2=2

for ar in "$@"
do
    res1=$(expr $ar + var1)
    res=$(expr $res1 / var2)
    echo $res
done

```

### Script 2 - pipes command in script

For this case we need to use tee because grep after grep not taking input directly from standard input, so tee is used to output ls output to filename1 and also to standard output which is passed to second grep. Without tee is not possible. We can ommit only $filename1 if we don't need to output first grep to file.

```
#!/bin/bash

catalog="/etc"
filename1="output1.txt"
filename2="output2.txt"

{
    ls $catalog -p | grep -v / | (tee $filename1) 2>&1 | grep -E 'am|al|ma' > $filename2
    # alternatively
    # ls $catalog -p | grep -v / | tee 2>&1 | grep -E 'am|al|ma' > $filename2
} 2>&1

```

### Script 3 - comparing values

```
number1=9
number2=4

if [ "$number1" -gt "$number2" ]; then
    printf("$number1 is greater then $number2")
fi
 ```

Other compares are:  
-le (less or equal)  
-eq (equal)  
-ge (greater or equal)  
-lt (less than)  
-ne (not equal)  

### Script 4 - using in for loops

```
number_of_files=`ls $kat -p | wc -l`

for ((counter=1; counter<=$number_of_files;counter++))
    do
        printf("$counter\n")
    done
```

### Script 5 - compare execution of command

Below code checks execution status of called program by ```"$? -eq 1"```  
If for example called program returns exit(1) then we know that was errored.  

```
if [[ $? -eq 1 ]]; then
    printf "Program returns via exit(1)\n";
else
    printf "something else\n";
fi
```