# Reference
<!-- DO NOT EDIT: This document was generated by Puppet Strings -->

## Table of Contents

**Functions**

* [`tahu::attributes`](#tahuattributes): Returns the instance attributes of an `Object` or `Type`.  An *instance attribute* is an attribute of a value. Only `Object` values have inst
* [`tahu::callable_signature`](#tahucallable_signature): Produces an `Array` of `Callable` signatures (parameters with types, and returned type) of functions.  Returns an `Array` with one or more `T
* [`tahu::convert_from_rich_data`](#tahuconvert_from_rich_data): Converts a value from a `RichData` compliant data structure to the actual runtime values.  This function is useful as deserialization of a ri
* [`tahu::convert_to_rich_data`](#tahuconvert_to_rich_data): Converts a value to a `RichData` compliant data structure.  This function is useful as serialization of rich data (data that cannot be direct
* [`tahu::eval`](#tahueval): Evaluates a string containing Puppet Language source and returns the result.  The primary intended use case is to combine `eval` with `Deferr
* [`tahu::get_attr`](#tahuget_attr): Returns the value of a given attribute of an Object or Type.  See * `tahu::attributes` for how to get instance attribute names * `tahu::type_
* [`tahu::is_callable_with`](#tahuis_callable_with): Answers if one of the given `Type[Callable]` can be called with the given arguments and optional block.  The function can be called with one 
* [`tahu::signature`](#tahusignature): Returns a data structure describing the type signature (parameters with types, and returned type) of "callable" entities.  The signature() fu
* [`tahu::stacktrace`](#tahustacktrace): Returns a full Puppet stacktrace.  This function returns an `Array[Tuple[String, Integer]]` where each tuple represents a call on the form `[
* [`tahu::to_type`](#tahuto_type): Converts a type in String format to an actual instance of Type.  This is useful when reading a data type from for example a YAML file and a r
* [`tahu::type_attributes`](#tahutype_attributes): Returns the type attributes of a `Type`  The returned value is a hash mapping attribute name to the data type for that attribute. If the give
* [`tahu::where`](#tahuwhere): Returns a tuple describing file, and line of the manifest location where this function was called.  When the call to `where()` is from a sour

## Functions

### tahu::attributes

Type: Ruby 4.x API

Returns the instance attributes of an `Object` or `Type`.

An *instance attribute* is an attribute of a value. Only `Object` values have
instance attributes; for example the `name` of a `Person`. Contrast this
with a *type attribute* which is an attributes of a `Type`. The type attributes
can be obtained with the function `tahu::type_attributes(T)` - as an example
the `Integer` type has the type attributes `to` and `from`, but integer values
for example `123` have no attributes - as it is just a value.

This function returns a hash mapping attribute name to the data type for that attribute.
If the given type (or type of Object) has no attributes an empty hash will be returned.

See
* `tahu::type_attributes` for how to get attributes of types
* `tahu::get_attr` for how to get the value of an attribute

#### Examples

##### Getting the attributes of a Type[Object]

```puppet
type MyThing = Object[attributes => { example => String }]
notice(tahu::attributes(MyThing)
# Would notice { "example" => String }
```

##### Getting the attributes of an Object

```puppet
type MyThing = Object[attributes => { example => String }]
$a_thing = MyThing("hello")
notice(tahu::attributes($a_thing)
# Would notice { "example" => String }
```

##### Getting the attribute names of a type results in an empty hash

```puppet
notice(tahu::attributes(Integer))
# Would notice `{}`
```

#### `tahu::attributes(Type[Object] $object_type)`

The tahu::attributes function.

Returns: `Hash[String, Type]`

##### `object_type`

Data type: `Type[Object]`

- The object type to get instance attributes from

#### `tahu::attributes(Type $type)`

The tahu::attributes function.

Returns: `Hash[String, Type]`

##### `type`

Data type: `Type`

- The type to get instance attributes from

#### `tahu::attributes(Object $an_object)`

The tahu::attributes function.

Returns: `Hash[String, Type]`

##### `an_object`

Data type: `Object`

- An object to get instance attributes from its type

### tahu::callable_signature

Type: Ruby 4.x API

Produces an `Array` of `Callable` signatures (parameters with types, and returned type) of functions.

Returns an `Array` with one or more `Type[Callable]` describing how a function can be called,
or how the `new` function of a data type can be called.

Data type signatures are produced as function signatures for the respective type's `new` function
except for `CatalogEntry` data types since they have different semantics and for which this function returns `undef`.
The value `undef` is also returned for non existing functions and for data types that do not have a `new` function.

See
* `tahu::is_callable_with` for testing if a callable can be called with a given set of arguments.
* `tahu::signature` to get more information about the signature as a data structure.

#### Examples

##### Getting the callable signature of a function

```puppet
notice(tahu::callable_signature("size"))
# Would notice: `[Callable[Variant[Collection, String, Binary]]]`
```

##### Getting the callable signature of a data type

```puppet
notice(tahu::callable_signature(Integer))
# Would notice: [Callable[Convertible, Radix, Boolean, 1, 3], Callable[NamedArgs]]
```

#### `tahu::callable_signature(String $function_name)`

The tahu::callable_signature function.

Returns: `Optional[Array[Type[Callable]]]`

##### `function_name`

Data type: `String`

- The name of a function to get `Callable` signature(s) from

#### `tahu::callable_signature(Type $type)`

The tahu::callable_signature function.

Returns: `Optional[Array[Type[Callable]]]`

##### `type`

Data type: `Type`

- The Type to get `Callable` signature(s) from its `new` function

### tahu::convert_from_rich_data

Type: Ruby 4.x API

Converts a value from a `RichData` compliant data structure to the actual runtime values.

This function is useful as deserialization of a rich data structure - for example something read from
a yaml file. This is the reverse of `tahu::convert_to_rich_data()`.

See
* `tahu::convert_to_rich_data` for how to serialize.
* [Pcore Data Representation Specification](https://github.com/puppetlabs/puppet-specifications/blob/master/language/data-types/pcore-data-representation.md)

#### Examples

##### Deserializing a value

```puppet
$r = /this is a regexp/
$serialized = tahu::convert_to_rich_data($r)
$deserialized = tahu::convert_from_rich_data($serialized)
notice( $deserialized == $r)
# would notice: true
```

#### `tahu::convert_from_rich_data(RichData $value)`

Converts a value from a `RichData` compliant data structure to the actual runtime values.

This function is useful as deserialization of a rich data structure - for example something read from
a yaml file. This is the reverse of `tahu::convert_to_rich_data()`.

See
* `tahu::convert_to_rich_data` for how to serialize.
* [Pcore Data Representation Specification](https://github.com/puppetlabs/puppet-specifications/blob/master/language/data-types/pcore-data-representation.md)

Returns: `Any`

##### Examples

###### Deserializing a value

```puppet
$r = /this is a regexp/
$serialized = tahu::convert_to_rich_data($r)
$deserialized = tahu::convert_from_rich_data($serialized)
notice( $deserialized == $r)
# would notice: true
```

##### `value`

Data type: `RichData`

- The rich data value to convert from

### tahu::convert_to_rich_data

Type: Ruby 4.x API

Converts a value to a `RichData` compliant data structure.

This function is useful as serialization of rich data (data that cannot be directly represented with YAML/JSON data
types alone) - for example writing the result to a yaml file for
later reading and deserialization into actual objects using `tahu::convert_from_rich_data()`.

See
* `tahu::convert_from_rich_data` for how to convert what this function returns back to runtime values
* [Pcore Data Representation Specification](https://github.com/puppetlabs/puppet-specifications/blob/master/language/data-types/pcore-data-representation.md)

* **Note** The "rich data format" is the format used in a Puppet catalog.

#### `tahu::convert_to_rich_data(Any $value)`

Converts a value to a `RichData` compliant data structure.

This function is useful as serialization of rich data (data that cannot be directly represented with YAML/JSON data
types alone) - for example writing the result to a yaml file for
later reading and deserialization into actual objects using `tahu::convert_from_rich_data()`.

See
* `tahu::convert_from_rich_data` for how to convert what this function returns back to runtime values
* [Pcore Data Representation Specification](https://github.com/puppetlabs/puppet-specifications/blob/master/language/data-types/pcore-data-representation.md)

Returns: `RichData`

##### `value`

Data type: `Any`

- The value to convert to `RichData`

### tahu::eval

Type: Ruby 4.x API

Evaluates a string containing Puppet Language source and returns the result.

The primary intended use case is to combine `eval` with
`Deferred` to enable evaluating arbitrary code on the agent side
when applying a catalog.

This function can be used when there is the need to format or transform deferred
values since doing that with only deferred values can be difficult to construct
or impossible to achieve when a lambda is needed.

The function accepts a Puppet Language string, and an optional `Hash[String, Any]`
with a map of local variables to make available when the string is evaluated - this
is how values can be passed from the location where a deferred `eval` is created
to the location where it will be resolved/evaluated.

Requires Puppet 6.1.0

#### Examples

##### Using `eval`

```puppet
tahu::eval("\$x + \$y", { 'x' => 10, 'y' => 20}) # produces 30
# Note the escaped `$` characters since interpolation is unwanted.

Deferred('tahu::eval' ["\$x + \$y", { 'x' => 10, 'y' => 20})] # produces 30 on the agent
```

##### Evaluating logic on agent requiring use of "filter"

```puppet
Deferred('tahu::eval', "local_lookup('key').filter |\$x| { \$x =~ Integer }")
```

##### Asserting the type of value produced by an eval is simply done by calling `assert_type`

```puppet
tahu::eval("assert_type(Integer, \$x + \$y))", { 'x' => 10, 'y' => 20})
```

#### `tahu::eval(String $code)`

The tahu::eval function.

Returns: `Any`

##### `code`

Data type: `String`

- Puppet Language source string to evaluate

#### `tahu::eval(String $code, Hash[String, Any] $variables)`

The tahu::eval function.

Returns: `Any`

##### `code`

Data type: `String`

- Puppet Language source string to evaluate

##### `variables`

Data type: `Hash[String, Any]`

- variable names (without $) to value map of local variables to set before evaluation

### tahu::get_attr

Type: Ruby 4.x API

Returns the value of a given attribute of an Object or Type.

See
* `tahu::attributes` for how to get instance attribute names
* `tahu::type_attributes` for how to get type attribute names

* **Note** It is only `Object` and `Type` values that have attributes.

#### Examples

##### Get attributes from a data type

```puppet
notice(Integer[10,100].tahu::get_attr('to'))
# Would notice: 100
```

##### Get attributes from an Object

```puppet
type MyThing = Object[attributes => {name => String}]
$a_thing = MyThing("banana")
notice($a_thing.tahu::get_attr('name'))
# Would notice: "banana"
```

##### Get attributes from a Type[Object]

```puppet
type MyThing = Object[attributes => {name => String}]
notice(MyThing.tahu::get_attr('attributes'))
# Would notice: {name => String}
```

#### `tahu::get_attr(Type[Object] $object_type, String $name)`

The tahu::get_attr function.

Returns: `Any` Any

##### `object_type`

Data type: `Type[Object]`

- The Object type from which an attribute is wanted (i.e. attributes that define an Object type)

##### `name`

Data type: `String`

- The name of the attribute

#### `tahu::get_attr(Type $type, String $name)`

The tahu::get_attr function.

Returns: `Any` Any

##### `type`

Data type: `Type`

- The Type from which an attribute is wanted (i.e. a type attribute/parameter)

##### `name`

Data type: `String`

- The name of the attribute

#### `tahu::get_attr(Object $value, String $name)`

The tahu::get_attr function.

Returns: `Any` Any

##### `value`

Data type: `Object`

- The Object from which an attribute is wanted

##### `name`

Data type: `String`

- The name of the attribute

### tahu::is_callable_with

Type: Ruby 4.x API

Answers if one of the given `Type[Callable]` can be called with the given arguments and optional block.

The function can be called with one `Type[Callable]` or an `Array[Type[Callable]]` (the later is
what is produced by `tahu::callable_signatures()`).

This function returns a `Boolean` indicating if the given function can be called or not with the given arguments and block.

#### Examples

##### Can size be called with an Array?

```puppet
tahu::callable_signature('size').is_callable_with([]).notice
# Would notice true
```

#### `tahu::is_callable_with(Type[Callable] $callable_t, Any *$arguments, Optional[Callable] &$block)`

The tahu::is_callable_with function.

Returns: `Boolean`

##### `callable_t`

Data type: `Type[Callable]`

- a Callable to check if it can be called

##### `*arguments`

Data type: `Any`

- none, one or more arguments to check if they can be used to call the `Callable`

##### `&block`

Data type: `Optional[Callable]`

- the function accepts an optional lambda that is used with Callable's that accepts lambdas

#### `tahu::is_callable_with(Array[Type[Callable]] $callables, Any *$arguments, Optional[Callable] &$block)`

The tahu::is_callable_with function.

Returns: `Boolean`

##### `callables`

Data type: `Array[Type[Callable]]`

- an Array of Callable to check if one of them can be called

##### `*arguments`

Data type: `Any`

- none, one or more arguments to check if they can be used to call the `Callable`

##### `&block`

Data type: `Optional[Callable]`

- the function accepts an optional lambda that is used with Callable's that accepts lambdas

### tahu::signature

Type: Ruby 4.x API

Returns a data structure describing the type signature (parameters with types, and returned type) of "callable" entities.

The signature() function can return a signature for:
* Functions
* Native resource types (for example `File`).
* Classes (i.e. created by `class` in Puppet Language).
* user defined resource types (i.e. created by `define` in Puppet Language).

*Function Signatures* are obtained by giving the function's name as a `String`.

*Data Type signatures* are produced as function signatures for the respective type's `new` function
except for `CatalogEntry` data types since they have different semantics.
For `CatalogEntry` data types a single signature is returned and it is possible to obtain it with or without meta parameters.

The `CatalogEntry` data types are:
* `Resource[<typename>]` - used to get the signature of a resource type. Short form aliases can be used, for example `File`.
* `Class[<classname>]` - used to get the signature of a class.

### Returned values

`Resource` and `Class` data types always have a single signature and use "named arguments".
Functions use "positional arguments", and they may have multiple signatures (multiple different sequences of positional parameters).
Functions also have "return type", support a repeating argument, and possibly a block (lambda).
Note that the signature for general data types are for the respective `new` function of a data type, and this signature
includes a variant where arguments are given as a struct - looking very much like an "named arguments" call.

For resources and classes the returned signature is a `Hash` with parameter names as keys, and each value being a hash of:
* `"type"` - `Type`, the type of the parameter
* `"optional"` - `Boolean`, set to `true` if value has a default value expression, (not present otherwise)
* `"meta"` - `Boolean`, set to `true` if value is a meta parameters - key not present for regular parameters

When the optional parameter `include_meta_params` is set to `true`, the meta parameters will also be included
in the result. This option defaults to `false`.

For functions (including general data type's `new` function), the returned information is an `Array` where each entry is
one signature. Each signature is a `Hash` with the keys:
* `"parameters"` - `Hash`, information about each parameter - the keys are the names of the parameters and each value is
  a hash with the keys `"type"` `Type`, and `"optional"` `Boolean`, and if the parameter is the last and repeating it will
  also have a `"repeating"` key set to `true`. The `"optional"` key is only present if the value is `true`.
* `"return_type"` - `Type`, the return type - is set to `Any` for functions that does not specify a return type
* `"block_type"` - `Type`, the type of a block if a block is accepted otherwise not present. When a function accepts a block
  the type is either a `Callable` if the block is required, or `Optional[Callable]` if the block is optional. The `Callable` data type
  further specifies the block's parameters and their types.

Since Puppet Language hashes have ordered entries, the hash also describe the position of each parameter.

The `signature()` function returns `undef` in case the given function name references a function that does not exist,
if a given data type does not exist, or if it does not have a `new` function.

*Related Information*
* To ask if a value can be used in a call to create a new instance of a type `T` use `$val =~ Init[T]`
* To ask if a `Callable` can be called with given values call `tahu::is_callable_with()`

#### Examples

##### Getting the signature of a function

```puppet
notice(tahu::signature("size"))
# Would notice: `[{parameters => {arg => {type => Variant[Collection, String, Binary]}}, return_type => Any}]`
```

##### Getting the signature of a class.

```puppet
class testing(Integer $x, String $y) { }
notice(tahu::signature(Class['testing']))
# Would notice `{x => {type => Integer}, y => {type => String}}`
```

##### Including meta parameters

```puppet
notice(tahu::signature(Class['testing'], true))
```

##### Getting the parameter-names of a class.

```puppet
tahu::signature(Class['testing'])['parameters'].keys()
```

##### Getting the signature of a general data type

```puppet
notice(tahu::signature(Integer))
# Would notice this quite long signature:
# [
#   { parameters => {
#       from  => {type => Convertible = Variant[Numeric, Boolean, Pattern[/\A[+-]?\s*(?:(?:\d+)|(?:0[xX][0-9A-Fa-f]+)|(?:0[bB][01]+))\z/], Timespan, Timestamp]},
#       radix => {type => Radix = Variant[Default, Integer[2, 2], Integer[8, 8], Integer[10, 10], Integer[16, 16]]},
#       abs   => {type => Boolean, optional => true}
#     },
#     return_type => Any
#   },
#   { parameters => {
#     hash_args => {
#       type => NamedArgs = Struct[{
#         'from'            => Convertible = Variant[Numeric, Boolean, Pattern[/\A[+-]?\s*(?:(?:\d+)|(?:0[xX][0-9A-Fa-f]+)|(?:0[bB][01]+))\z/], Timespan, Timestamp],
#         Optional['radix'] => Radix = Variant[Default, Integer[2, 2], Integer[8, 8], Integer[10, 10], Integer[16, 16]],
#         Optional['abs']   => Boolean
#     }]}},
#     return_type => Any
#   }
# ]
```

#### `tahu::signature(String $function)`

The tahu::signature function.

Returns: `Optional[Array[Hash]]`

##### `function`

Data type: `String`

- the name of the function to get signature(s) from

#### `tahu::signature(Variant[Type[CatalogEntry], Type[Type[CatalogEntry]]] $entity, Optional[Boolean] $include_meta_params)`

The tahu::signature function.

Returns: `Optional[Hash]`

##### `entity`

Data type: `Variant[Type[CatalogEntry], Type[Type[CatalogEntry]]]`

- the CatalogEntry (class or resource) Type to get a signature from

##### `include_meta_params`

Data type: `Optional[Boolean]`

- optional flag that when set to true will include the meta parameters of the entity

#### `tahu::signature(Type $type)`

The tahu::signature function.

Returns: `Optional[Array[Hash]]`

##### `type`

Data type: `Type`

- the Type for which signature(s) are to be produced for its `new` function

### tahu::stacktrace

Type: Ruby 4.x API

Returns a full Puppet stacktrace.

This function returns an `Array[Tuple[String, Integer]]` where each tuple represents a call on the form
`[<file>, <line>]`. The first entry in the array is the location calling the `stacktrace()` function.

If "file" is not know (for example when called from the command line) it
is set to "unknown".

Also see `tahu::where()` for getting only the top of the stack (which is much faster than getting the entire stack
and extracting only the immediate caller).

* **Note** (This function uses Puppet's Puppet::Pops::PuppetStack Ruby API. If something is not showing in the stack
then this is a problem in Puppet, not in this function).

#### Examples

##### unknown location

```puppet
puppet apply -e 'notice(tahu::stacktrace())'
# Would produce: [[unknown, 1]]
```

#### `tahu::stacktrace()`

Returns a full Puppet stacktrace.

This function returns an `Array[Tuple[String, Integer]]` where each tuple represents a call on the form
`[<file>, <line>]`. The first entry in the array is the location calling the `stacktrace()` function.

If "file" is not know (for example when called from the command line) it
is set to "unknown".

Also see `tahu::where()` for getting only the top of the stack (which is much faster than getting the entire stack
and extracting only the immediate caller).

Returns: `'Array[Tuple[String, Integer]]'` Array[Tuple[String, Integer]]'

##### Examples

###### unknown location

```puppet
puppet apply -e 'notice(tahu::stacktrace())'
# Would produce: [[unknown, 1]]
```

### tahu::to_type

Type: Ruby 4.x API

Converts a type in String format to an actual instance of Type.

This is useful when reading a data type from for example a YAML file and
a real data type is wanted to for example match a value against the read data type.

#### Examples

##### Creating an integer type

```puppet
$int_type = tahu::convert_string_to_type("Integer[1,2]")
notice( 2 =~ $int_type)
# Would notice: true
```

#### `tahu::to_type(String $type_string)`

Converts a type in String format to an actual instance of Type.

This is useful when reading a data type from for example a YAML file and
a real data type is wanted to for example match a value against the read data type.

Returns: `'Type'` Type'

##### Examples

###### Creating an integer type

```puppet
$int_type = tahu::convert_string_to_type("Integer[1,2]")
notice( 2 =~ $int_type)
# Would notice: true
```

##### `type_string`

Data type: `String`

- a puppet data type in string form for which a real runtime data type is wanted

### tahu::type_attributes

Type: Ruby 4.x API

Returns the type attributes of a `Type`

The returned value is a hash mapping attribute name to the data type for that attribute.
If the given type (or type of Object) has no attributes an empty hash will be returned.

* **Note** For `Type[Object]` values the returned attributes are for the object *type* not the attributes
of instances of that type. Use `tahu::attributes` to get the *instance attributes*.

#### Examples

##### Getting the type attribute names of a `Type`

```puppet
tahu::type_attributes(Integer).keys.notice
# Would notice ["to", "from"]
```

##### Getting the type attributes of a `Type[Object]`

```puppet
type MyThing = Object[attributes => { example => String }]
tahu::attributes(MyThing).keys
# Would notice [name, parent, type_parameters, attributes, constants, functions, equality, equality_include_type, checks, annotations]
# Note that 'example' is an instance attribute and it is not included
```

##### Getting the attributes of an `Object`

```puppet
type MyThing = Object[attributes => { example => String }]
$a_thing = MyThing("hello")
notice(tahu::attributes($a_thing)
# Would notice { "example" => String }
```

#### `tahu::type_attributes(Type[Object] $object_type)`

The tahu::type_attributes function.

Returns: `Hash[String, Type]`

##### `object_type`

Data type: `Type[Object]`

- The Type[Object] to get type attributes from

#### `tahu::type_attributes(Type $type)`

The tahu::type_attributes function.

Returns: `Hash[String, Type]`

##### `type`

Data type: `Type`

- The Type to get type attributes from

### tahu::where

Type: Ruby 4.x API

Returns a tuple describing file, and line of the manifest location where this function was called.

When the call to `where()` is from a source string provided via an API or command line, the "file" entry will
be set to the string "unknown".
In case the function is not called from a puppet manifest, an array of `[undef, undef]` will
be returned.

#### `tahu::where()`

Returns a tuple describing file, and line of the manifest location where this function was called.

When the call to `where()` is from a source string provided via an API or command line, the "file" entry will
be set to the string "unknown".
In case the function is not called from a puppet manifest, an array of `[undef, undef]` will
be returned.

Returns: `Tuple[Optional[String], Optional[Integer]]`

