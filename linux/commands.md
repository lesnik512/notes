### Working with dirs
```bash
cd -      # Move to previous directory
ls -a     # show hidden files
ls -l     # more info
ls -lh    # more info and clearer size of files
mkdir -p  # recursive creating of dirs
```
### Working with files
```bash
cat > 1.txt # create file and write data in it (to stop press ctrl+D)
tail -f     # update output if file is changed
```
### Control operators
```bash
sleep 20 &  # do not wait till the end
echo $      # print response code of previous command
cd r && ls  # && - "AND" operator
cd r || ls  # || - "OR" operator
```
### Shell embedding
```bash
echo $(var1=5;echo $var1) # multi level embedding
echo `var1=5;echo $var1`
```
### Filters
```bash
cut -d: -f1,3 /etc/passwd | tail -4  # split line by ":" and take 1st and 3rd items in last 4 lines
cat tennis.txt | tr 'a-z' 'A-Z'      # uppercase
cat tennis.txt | tr -d e             # remove symbol "e"
wc tennis.txt                        # count lines, words and symbols
sort -n -k3 country.txt              # sort by 3rd column numerically
sort music.txt |uniq                 # sort and remove duplicates
sort music.txt |uniq -c              # same as above, but + counting duplicates
```