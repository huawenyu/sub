# Doc

https://github.com/lonkamikaze/bsda2/blob/master/ref/lst.md

# basic array
LST.sh is a simple portable shell array library. It provides a way of treating a string as an array by splitting it using a Record Separator character.

```sh
source ./bsda2-arr

rec a= 'First entry' $'Second\nentry' 'Third entry$'

  # Arrays can be accessed by index:
rec a[1]      # prints: First entry
rec a[-1]     # prints: Third entry$
rec a[2] entry      # assign to a new var
test "${entry}" = $'Second\nentry' # succeeds

echo "a-count=$( rec a.count )"
echo "a-index2='$( rec a[2].get )'"


  # Entries can be appended or prepended:
rec a.push_back  'Final entry'
rec a.push_front 'Preliminary entry'

  # Providing the Record Separator RS to lst()
ORS=\| rec a  # prints the whole array
              # Preliminary entry|First entry|Second
              # entry|Third entry$|Final entry

  # Entries can be appended or prepended:
i=0
while rec a[i+=1] entry; do
    printf '%2d: <%s>\n' $i "${entry}"
done          # prints:
              #  1: <Preliminary entry>
              #  2: <First entry>
              #  3: <Second
              # entry>
              #  4: <Third entry$>
              #  5: <Final entry>

  # Or by regular variable expansion:
rec a.set_ifs
for entry in $a; do
    echo "<${entry}>"
done          # prints:
              # <Preliminary entry>
              # <First entry>
              # <Second
              # entry>
              # <Third entry$>
              # <Final entry>
```

# implement comp by parse args


```sh
source ./bsda2-arr

declare -A args_def=(\
    [--action]="create push delete" \
    [--branch]="" \
    [--message]="" \
    [--test]="" \
    )

rec a= 'First entry' $'Second\nentry' 'Third entry$'

#str="--action\ create\ --branch\ feature1\ --message='fix bugs'\ --action"
#str="--action\ create\ --branch\ feature1\ --message='fix bugs'\ "
str="--complete"
arr=(${str//\\ / })
arr_num=${#arr[@]}

the1st=$( arr.peek_front arr )
echo "The front='$the1st' end='$( arr.peek_back arr )'"

arr.compleat args_def arr
```

