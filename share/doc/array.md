# array

https://linuxize.com/post/bash-arrays/
https://opensource.com/article/18/5/you-dont-know-bash-intro-bash-arrays

```sh
distro=("redhat" "debian" "gentoo")

 ## get length of $distro array
len=${#distro[@]}

 ## Iterate array's value
for (( i=0; i<$len; i++ )); do
    echo "${distro[$i]}"
done

 ## Iterate array's key, special to the associative-array
declare -A PERSONS
declare -A PERSON

PERSON["FNAME"]='John'
PERSON["LNAME"]='Andrew'
i=1
for key in "${!PERSON[@]}"; do
  PERSONS[$i,$key]=${PERSON[$key]}
done

PERSON["FNAME"]='Elen'
PERSON["LNAME"]='Murray'
((i++))
for key in "${!PERSON[@]}"; do
  PERSONS[$i,$key]=${PERSON[$key]}
done

declare -p PERSONS
```



```sh

 #Array values,length,number of elements
os=('ubuntu' 'mac' 'windows')
echo "type-os=$(declare -p os)"

 #Add values into array for particular index
os[5]='fedora'
os[6]='centos'

 #Add values without specifying index
os+=('msdos')

 #To remove value at particular index
 #unset os[6]

 #To print first element of array
echo "array-idx-0=${os[0]}"
 #Print value assgigned to a index in array where index isn't present
echo "array-idx-4=${os[4]}"
 #To print all indexes of array
echo "array-indexs=${!os[@]}"
 #To print entire array
echo "array-value=${os[@]}"
 #To print number of element in a array
echo "array-count=${#os[@]}"


declare -A matrix
echo "type-matrix=$(declare -p matrix)"

matrix[0,0]="${os[@]}"

 # test if associative array is declared
[[ -v matrix[@] ]] && echo "matrix set"

matrix[0,1]="B"
matrix[0,2]="C"
matrix[1,0]="D"
matrix[1,1]="E"
matrix[1,2]="F"

echo "matrix=${matrix[@]}"
 # https://stackoverflow.com/questions/3685970/check-if-a-bash-array-contains-a-value
value=A; echo -n "matrix contain '${value}': "; [[ " ${matrix[@]} " =~ " ${value} " ]] && echo "YES" || echo "NO";
value=D; echo -n "matrix contain '${value}': "; [[ " ${matrix[@]} " =~ " ${value} " ]] && echo "YES" || echo "NO";

new_os=(${matrix[0,0]})
echo "type-new_os=$(declare -p new_os)"
echo "new_array-count=${#new_os[@]}"

new_os2=${matrix[0,0]}
echo "type-new_os2=$(declare -p new_os2)"
echo "new_array-count2=${#new_os2[@]}"


 #str="--action\ create\ --branch\ feature1\ dir=/tmp/log\ "; arr=(${str//\\ / }); echo "#=${arr[#]}"; echo ${arr[@]}
str="--action\ create\ --branch\ feature1\ dir=/tmp/log\ "
arr0=($str)
echo "type-arr0=$(declare -p arr0)"
echo "arr0-count2=${#arr0[@]}"

arr=(${str//\\ / })
echo "type-arr=$(declare -p arr)"
echo "arr-count2=${#arr[@]}"
arr1=(${str//\\/ })
echo "type-arr1=$(declare -p arr1)"
echo "arr1-count2=${#arr1[@]}"
```

# simulate 2d arrays

https://www.baeldung.com/linux/bash-2d-arrays

# pass array to function

```sh
 #pass an array by name reference to a function in bash (since version 4.3+), by setting the -n attribute:
show_value () # array index
{
    local -n myarray=$1
    local idx=$2
    echo "${myarray[$idx]}"
}

shadock=(ga bu zo meu)
 #pass by reference, don't add $ like $shadock
show_value shadock 2
```

# delete item from array

https://stackoverflow.com/questions/16860877/remove-an-element-from-a-bash-array

```sh
 #Note: the new array has gaps, the indices will no longer be a continuous sequence of integers.
array=(pluto pippo bob)
delete=(pippo)
for target in "${delete[@]}"; do
  for i in "${!array[@]}"; do
    if [[ ${array[i]} = $target ]]; then
      unset 'array[i]'
    fi
  done
done

declare -p array

 #Note: avoid the gaps, we can just assign empty string as the new value
array=(pluto pippo bob)
delete=(pippo)
for target in "${delete[@]}"; do
  for i in "${!array[@]}"; do
    if [[ ${array[i]} = $target ]]; then
      #unset 'array[i]'
      array=("${array[@]/$target}")
    fi
  done
done

declare -p array
```

