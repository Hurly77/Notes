```js
function mapToNegativize(sourceArray){
  let n = []
  for (let i = 0; i < sourceArray.length; i++){
    n.push(sourceArray[i] * -1)
  }
  return n
}

function mapToNoChange(sourceArray){
  let n = []
  for(let i = 0; i < sourceArray.length; i++){
    n.push(sourceArray[i])
  }
  return n
}

function mapToDouble(sourceArray){
  let n = []
  for(let i = 0; i < sourceArray.length; i++){
    n.push(sourceArray[i] * 2)
  }
  return n
}

function mapToSquare(sourceArray){
  let n = []
  for(let i = 0; i < sourceArray.length; i++){
    n.push(sourceArray[i] * sourceArray[i])
  }
  return n
}

function reduceToTotal(src, start=0) {
  let total = start
  for (let i = 0; i < src.length; i++ ) {
    total = total + src[i]
  }
  return total
}

function reduceToAllTrue(src) {
  for (let i = 0; i < src.length; i++ ) {
    if (!src[i]) return false
  }
  return true
}

function reduceToAnyTrue(src){
  for(let i = 0; i < src.length; i++){
    if(src[i]) return true
  }
  return false
}
```

