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
# Arrays
# Create array
## Empty array
```c
VALUE ary = rb_ary_new();
rb_ary_push(ary, VALUE);
// ...
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
