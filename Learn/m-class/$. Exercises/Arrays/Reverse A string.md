# Reverse String

The Interviewer is asking to reverse a string.

```js
//hi my name is cameron
//should be: 'noremac si eman ym ih'

const Reverse = (str) => {
    
}
```

# two sum

Given `nums` and array of number and *n* the target find the two indices that add up to *n*

checks

- selection can't be bigger than *n*
- check for *target* and 0
- only elms between 0 and *target* are valid
- if the selection is 

possible approaches

-  compare each element to the rest of the array 0(n*2)
- Do the above but be for each loop run checks on the element.
- create a hash map, one the first pass mark if the `target` is in the array, if 0 is also in the array return those incise

# **attempt one**  

steps

1. map through array check for 0 and *target* set variable the equal there index
2. name each key the value at `idx` and the value of the key the current `idx`
3. check for the closes value between 0 and `target` 
4. if both 0 and `target` are in the array return those values
5. in the second loop 