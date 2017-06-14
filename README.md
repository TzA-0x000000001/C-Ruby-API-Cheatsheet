# C Ruby API Cheatsheet
A list of usefull codes and tips for C Ruby api.

# Structs
## Load wrapped struct
```c
MyStruct * data;
Data_Get_Struct(self, MyStruct, data);
```
## Create wrapped struct
```c
MyStruct * data = malloc(sizeof(MyStruct));
return Data_Wrap_Struct(self, FUNC_MyStruct_mark, FUNC_MyStruct_free, data);
```

# Methods
## No args
```c
rb_define_method(rb_cKlass, "method", FUNC_KLASS_method, 0);

VALUE FUNC_KLASS_method(VALUE self) {
    
    MyStruct * data;
    Data_Get_Struct(self, MyStruct, data);
    
    // Do something
    
    return VALUE;
    
}
```
## Fixed number of args
```c
rb_define_method(rb_cKlass, "method", FUNC_KLASS_method, 2);

VALUE FUNC_KLASS_method(VALUE self, VALUE arg1, VALUE arg2) {
    
    MyStruct * data;
    Data_Get_Struct(self, MyStruct, data);
    
    // Do something
    
    return VALUE;
    
}
```
## Optional arguments
```c
rb_define_method(rb_cKlass, "method", FUNC_KLASS_method, -1);

VALUE FUNC_KLASS_method(int argc, VALUE* argv, VALUE self) {
    
    MyStruct * data;
    Data_Get_Struct(self, MyStruct, data);
    
    VALUE arg1, arg2, arg3, arg4;
    
    rb_scan_args(argc, argv, "13", &arg1, &arg2, &arg3, &arg4); // 1 normal argument and 3 optional arguments
    
    if(NIL_P(arg2))
        // Don't forget to check optional arguments
    
    // Do something
    
    return VALUE;
    
}
```
# Types
## Check types
```c
NIL_P(VALUE); // nil
FIXNUM_P(VALUE); // fixnum
RB_FLOAT_TYPE_P(VALUE); // float
SYMBOL_P(VALUE); // symbol
RTEST(VALUE); // true or false
RB_TYPE_P(VALUE, T_TYPE); // other builtin types
RBASIC_CLASS(VALUE) == rb_cKlass // other classes
```
## Usefull macros
```c
#define RB_CHECK_KLASS(o,k) (RBASIC_CLASS(o) == k)
```
# Arrays
# Create array
## Empty array
Creates an empty array with size 0.
```c
VALUE ary = rb_ary_new();
rb_ary_push(ary, VALUE);
// ...
```
Creates an empty array with a default size.
```c
int length = 10;
VALUE ary = rb_ary_new2(length);
// ...
```
## Array with default values
Creates an array of defined VALUEs.
```c
int length = 2;
VALUE ary = rb_ary_new3(length, VALUE el1, VALUE el2);
// ...
```
Creates an array from a C array of VALUEs.
```c
int length = 2;
VALUE ary_c[2] = {VALUE, VALUE};
VALUE ary = rb_ary_new4(length, ary_c);
// ...
```
## Array duplication
```c
VALUE ary1 = rb_ary_new();
VALUE ary2 = rb_ary_dup(ary1);
```
# Array manipulation
## Get array length
```c
RARRAY_LEN(VALUE) # returns length in C
```
## Setting array values
Stores a VALUE in an array at defined index. Note that negative indexes count from the end of the specified array.
```c
rb_ary_store(VALUE ary, long index, VALUE element);
```
Appends an element at the end of the array.
```c
rb_ary_push(VALUE ary, VALUE element);
```
Prepends an element at the start of the array.
```c
rb_ary_unshift(VALUE ary, VALUE element);
```
## Getting array values
Gets the element at defined index. Note that negative indexes count from the end of the specified array.
```c
rb_ary_entry(VALUE ary, long index);
```
## Deleting array values
Removes the last element of the array.
```c
rb_ary_pop(VALUE ary);
```
Removes the first element of the array.
```c
rb_ary_shift(VALUE ary);
```
Removes the element of the array at defined index. If index is out of bounds, returns `Qnil`. Otherwise, returns the deleted element. Note that negative indexes count from the end of the specified array.
```c
rb_ary_delete_at(VALUE ary, long index);
```
Removes all the elements in the array that are equal to specified item. Returns `Qnil` if no matches are found. Note that the item can be a block.
```c
rb_ary_delete(VALUE ary, VALUE item_to_delete);
rb_ary_delete(VALUE ary, VALUE block);
```
Deletes all elements from the array.
```c
rb_ary_clear(VALUE ary);
```
# Symbols
## Create symbol
### From string
```c
VALUE sym = ID2SYM(rb_intern("symbol"));
```
### From VALUE
```c
VALUE sym = ID2SYM(rb_intern_str(VALUE));
```
