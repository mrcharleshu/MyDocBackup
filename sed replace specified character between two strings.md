[来自stackexchange](http://unix.stackexchange.com/questions/152621/replace-specified-character-between-two-strings)

### sed只能替换一个red

```
echo "blue red from red blue red until blue red" | sed 's/\(from.*\)red\(.*until\)/\1??\2/g;'
```
或者出现多次，但每次也只能替换一个red

```
echo "blue red from red blue red until red from red blue red until blue red" | sed -e :1 -e 's@\(from[^(until)]*\)red\(.*until\)@\1??\2@;t1’
```

### perl能替换两个red

```
echo "blue red from red blue red until blue red" | perl -pe 's%(from.*?until)% $1 =~ s/red/??/gr %eg'
```
或者出现多次

```
echo "blue red from red blue red until red from red blue red until blue red" | perl -pe 's%(from.*?until)% $1 =~ s/red/??/gr %eg'
```
> Explanation: take everything between the <ex> tags into $1, and inside $1, replace & by #.
