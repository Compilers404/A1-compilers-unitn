## FOR

**Introducing a new variable**
```
for i: int = 0; i < n; i = i + 1 { ... }
--> 
{    
    int i = 0;
    while(i<n){
        ...
        i = i + 1;
    }
}
```
The block is to introduce a "fresh" variable. 
The user can not declare a for with an index i if another i is already in scope: this ensures we do create collisions. 

**Using an existing variable**
```
int j = 4;
j = j + 2;
for j = 0; j < n; j = j + 1 { ... }
--> 
j = 0;
while(j<n){
    j = j + 1;
}

for ; j < n; j = j + 1 { ... }
--> 
while(j<n){
    ...
    j = j + 1;
}

for j=0; ; j = j + 1 { ... }
--> 
j = 0;
while(true){
    ...
    
    j = j + 1;
}

for j=0; j < n;; {...}
-->
j = 0;
while(j < n){

}
```


## DO WHILE

```
do { c } while (e)
--> 
{ // this braces are needed to not keep in scope the variables declared inside c
    c // one iteration, executed regardless the guard
    while(e){
        c
    }
}

do{
    x: int = 0;
    x = x + 1;
}while (x < 10);

```


## Things to consider
1. Semicolons are not mandatory: include tests
2. Statement can be on the same line without semicolons: test!
3. if break/continue instructions, or jumps to labels, were allowed, this wouldn't hold. Ensure they are not allowed
4. For do-while, add test where e is not inside braces? 
5. Should we write tests that should fail (not to hand them over)?