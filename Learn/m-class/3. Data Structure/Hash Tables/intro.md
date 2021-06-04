**idempotent** - denoting an element of a set which is unchanged in value when multiplied or otherwise operated on by itself.

# What are Hash Tables and How do we use them?

If your familiar with JavaScript You’ve more than likely call hash tables Objects, in ruby the call them hashes and in python they call the dictionaries. Though how is this achieved? We are able to create objects such as one below.

```js
//JavaScript syntax
const myObject = {
    address: '126.001.01.1'//not a real address just random digits
}
```

by using a hash function. To put it simply a hash function digests the syntax and allocates memory with an associated address know has the key and the value that the key holds.

when your want to set a key let `myObject.name` a hash function will generate something that looks like this `18697449d7c48cf32cdd4f14857e68ee`. There is no way to undo this or change it in anyway. This process in know as **idempotent** which accentually means that given the same input will have the same output. You can test this your self if you want. go [here](https://www.miraclesalad.com/webtools/md5.php) and select the md5 hash generator and type in `name` and I guarantee the out put will be `18697449d7c48cf32cdd4f14857e68ee`. The reason why this is so helpful is because we unlike arrays we don’t need to iterate over anything to retrieve the value. We simple just access the object we want and the assonated key to that value. which will give us really fast output.

Hash tables can be really useful when writting algorithms lets say we have to compare to arrays and we want to check if any elements in and array exist inside another array for examle.

```js
// check to see if one array is in another array.
const arrOne = ["a", "b", "c", "d", "e"]
const arrTwo = [1, "b", 2, 3, 4]
//we want to return true if any elments from array one exist inside array two.

//let define a funtion that can solve this problem

const compare = (arr1, arr2) => {
  let hashMap = {}// set an empty object
for(let i = 0; i < arr1.length; i++){
      if(!hashMap[arr1[i]]){
          hashMap[arr1[i]] = true
      }
  }

for(let j = 0; j < arr2.length; j++){
      if(hashMap[arr2[j]]){
          return true
      }
  }
return false
}

compare(arrOne, arrTwo)
//=> true
```

This allows use to minimize the amount of operations our function will execute. The big-O of this would be `O(n)` because as our data increase the number of operation to carry out our function will also increase. If we didn’t use a `hashMap` as I’m calling it. We’d have to create a nested for loop that would increase the number of operations to `O(n*2)` or `O(a*b)` depending on that size of each array.

Hopefully this helps clarify you understanding of what hash tables are and how we can use them. If you struggling with data Structures and want to learn how you can get better at using them. I’d Highly recommend that you check out [Andrei Neagoie’s](https://www.udemy.com/course/master-the-coding-interview-data-structures-algorithms/#instructor-1) course on Mastering the coding interview. This is a really good resource and It helped me a lot. He goes into detail about most the topics that I blog about and is one of my primary sources for education on data Structures. 