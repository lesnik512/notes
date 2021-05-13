# Commands
### Working with dirs
```bash
cd -      # Move to previous directory
ls -a     # show hidden files
ls -lh    # more info and clearer size of files
mkdir -p  # recursive creating of dirs
```
### Control operators
```bash
echo $      # print response code of previous command
cd r && ls  # && - "AND" operator
cd r || ls  # || - "OR" operator
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

# Solutions
## Remove duplicates from zsh history

```
setopt EXTENDED_HISTORY
setopt HIST_EXPIRE_DUPS_FIRST
setopt HIST_IGNORE_DUPS
setopt HIST_IGNORE_ALL_DUPS
setopt HIST_IGNORE_SPACE
setopt HIST_FIND_NO_DUPS
setopt HIST_SAVE_NO_DUPS
setopt HIST_BEEP
```