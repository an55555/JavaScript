# THE FUNCTION TYPE

> Function are object,function name are simply pointer to function object

### Callee

Callee is a pointer to the function that owns the arguments object
 
 ```javascript
function factorial(num){
    if(num<=1){
        return 1;
    }else{
        return num*factorial(num-1);
    }
}
```

In this factorial function, the proper execution of this function is tightly coupled width the function
 name "factorial".It can be decoupled by using argument.callee as follow:
 
  ```javascript
 function factorial(num){
     if(num<=1){
         return 1;
     }else{
         return num*arguments.callee(num-1);
     }
 }
 ```
 
 ### Caller
 
 Caller contains to the function that called this function or null if the function was called frome
 the global scope.For example
 
 ```javascript
function outer(){
    inner();
}
function inner(){
    alert(arguments.callee.caller)//outer()
}
```