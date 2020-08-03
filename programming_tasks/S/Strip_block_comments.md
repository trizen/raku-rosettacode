[1]: https://rosettacode.org/wiki/Strip_block_comments

# [Strip block comments][1]

```perl
sample().split(/ '/*' .+? '*/' /).print;
 
sub sample {
'   /**
    * Some comments
    * longer comments here that we can parse.
    *
    * Rahoo
    */
    function subroutine() {
     a = /* inline comment */ b + c ;
    }
    /*/ <-- tricky comments */
 
    /**
     * Another comment.
     */
    function something() {
    }
'}
```


Output:


#### Output:
```
   
    function subroutine() {
     a =  b + c ;
    }
    

    
    function something() {
    }
```