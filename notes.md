
for i: int = 0; i < n; i = i + 1 { ... }
--> 
int i = 0;
while(i<n){
    ...
    i = i + 1;
}


do { c } while (e)
--> 
c // one iteration 
while(e){
    c
}