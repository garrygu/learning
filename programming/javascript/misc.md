
- Accidental global variables  
```
function foo(){
    let a=b=0;
    a++;
    return a;
}
foo() //1
typeof a;   //"undefined"
typeof b;   //"number"
```

- What's the value of clothes[0]  
```
const clothes=['jacket','t-shirt'];    //undefined
clothes.length=0;  //0
clothes[0]  //undefined
```

- for() cycles over a null statement
```
for (var i=0;i<length;i++)
;{
    numbers.push(i+1);
}

numbers;  //[5]
```

Ref:
- http://0.30000000000000004.com/
