Every process has three streams: **stdin** (0), **stdout** (1), **stderr** (2). The pipe `|` connects one process's stdout to the next process's stdin. That's it — that's the entire Unix philosophy in one character.
```
# stdout of cat → stdin of grep 
cat /var/log/syslog | grep "error"
```
**Redirection** controls where streams go instead of the terminal:
```
>  # stdout to file (overwrites) 
>>  # stdout to file (appends) 
2>  # stderr to file 
2>&1  # redirect stderr to wherever stdout is going
```
