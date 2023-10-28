Hello, I'm creating a new language "patlang" which uses a new kind of type system. In this type system types (also refered to as patterns) are expressed as strings which can be compared using basic string comparison. Patterns consist of Unicode letters (which are ignored except for comparison purposes) and "holes" which themselves have types. A hole is any "word" which consists of the name of the hole a : and then the type of the hole. Here is an example pattern in patlang:

```
pat rectangle width:float mm  x height:float mm
```

Before being compared types are canonlicalized into their canonical string representation. In order to canonicalize types any whitespace is shrunk to a single space and non-base patterns are resolved. The canonical form of the rectangle pattern is `width:float mm x height:float mm`

If there is a second pattern:

```
pat box base:rectangle x depth:float mm
```

Then in this case the `rectangle` pattern has to be resolved during the pattern canonicalizaton process as unlike "float", `rectangle` is not a base type. Thus when we canonicalize box we end up with "width:float mm x height:float mm x depth:float mm".

All white space is shrunk to a single space when patterns are canonicalized new lines are removed:

```
pat krabice width:float mm x
                  height:float mm x
                  depth:float mm
```

Should canonicalize to the same pattern as the `box` pattern.

Patterns can contain `||` symbols. This is a union type. The subpatterns of a union type are considered to be separate patterns. The `||`ed patterns should be sorted alphabetically during canonicalization.

```
pat choice C weight:int || B foo:int || A bar:int
```

Would canonicalize to: `A bar:int || B foo:int || C weight:int`

There is a special case for recursive patterns. For example:

```
pat int_list item:int rest_of_list:int_list || end of list
```

Would be canonicalized as

```
int_list
@ intlist end of list || item:int rest_of_list:int_list
```

In this example, we are using the @ to represent a non fully resolvable subpattern. If we were to try to resolve this recursive pattern, then the canical form woud expand indefinitely.

Finally, there is the possibility for patterns to have type parameters (which are placed in carrots):

```
pat list<a> item:a rest_of_list:list<a> || end of list
```

In this case the canonical form would be:

```
list<a>
@ list<a> end of list || item:a rest_of_list:list<a>
```

Can you please try to write a haskell function which given a block of text with multiple patterns, and a pattern name, returns the canonicalized form for that pattern?

get_canonical_pattern :: String -> String -> String
get_canonical_pattern pattern_declarations pattern_name = <code to get canonical form for patter_name>
