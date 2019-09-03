---

layout: post
title: "Shell Skills (continuously update...)"
date: 2017-03-02 13:00:00 +0800

---

If there's no specification, it refers to BASH.

## snippets

* get the current directory of THIS executing script

```shell
DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
```

## about array

[reference](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_10_02.html)

* creating array

```shell
ARRAY[INDEXNR]=value

declare -a ARRAYNAME

ARRAY=(value1 value2 ... valueN)

ARRAYNAME[indexnumber]=value
```

* accessing array

```shell
ARRAY=(one two three)

echo ${ARRAY[*]}
one two three

echo $ARRAY[*]
one[*]

echo ${ARRAY[2]}
three

ARRAY[3]=four

echo ${ARRAY[*]}
one two three four
```

* deleting array [variables]

using ```unset``` command

```shell
unset ARRAY[1]

echo ${ARRAY[*]}
one three four

unset ARRAY

echo ${ARRAY[*]}
<--no output-->
```