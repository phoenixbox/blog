## Overview

#### Intro
The most recent Apple WWDC saw the introduction of the company's new programming language Swift, to much interest by the developer commnuity. The question on many minds: 'Will this replacement for Objective-C make it easier for me to get into navtive iOS development?'. 

Objective-C's verbose syntax is often the subject of much developer ire, leading to many mobile web-developers deciding to forgo native and opt for either responsive web development or a hybrid approach with PhoneGap/Titanium.

Objective-C's verbosity belies the awesome power and fun of developing on iOS, and in truth, whilst it is verbose, in many ways its not too disimmilar from other OOP languages like Ruby.

Swift's more terse syntax could be the happy medium developers are looking for. In this series of blog posts, I will compare common syntactical elements of each language, Ruby, Objective-C & Swift to give you a feel for they compare.

My goal, to tear down the pre-conceptions of the cons related to Objective-C and show how Swift improves upon them.

#### Variables - Constants - Strings - Interpolation
###### Ruby
Ruby, a dynamically typed language uses type inference. We can just assign an object to a string and then it is of type String. Easy-peasy, no need for a **var** declaration like in javascript, for example.

We declare constants with uppercase chars. To interpolate a string with a variable we use the #{var} syntax.

```
// vars_constants_strings_interpolation.rb
FRAMEWORK = "Rails"
language = "Ruby"
puts "Our #{language} example."
puts "Ruby on #{FRAMEWORK}."
```

###### Objective-C
Shield your eyes, this is where things get a tiny bit verbose. With Objective-C we declare type when defining our objects. We use \#define to declare constants. For a String we preceed the var name with its type, NSString and a * pointer. For string interpolation we use a %@ string format specifier and pass in the object as a parameter. There are various string format specifiers for different types. For instance, a primitive type integer requires %d.

```
// VarsConstatnsStringsInterpolationObjC.m
#define IDE @"xCode"
NSString *language = @"Objective-C";
NSLog(@"Our %@ example", language); // "Our Objective-C example"
NSLog(@"Our IDE is %@", IDE); // "Our IDE is xCode"
int number = 1;
NSLog(@"The number %d", number); // The number 1
```

But wait, pointer? What? * is the **dereference operator**. When we use *varName it means "Get me the thing that the pointer varName points to". We do this to tell the compiler that we intend to change the thing stored in memory that varName points to.

Ugghhhh.... I have to worry about memory managment? Nope! As of Xcode +4.2, Mac OS X +10.6 and iOS +4.0 there is automatic memory management (ARC), just like Ruby! Gone are the days when you had to do manual memory management with Objective-C and iOS. **ARC** (Automatic Reference Counting) takes care of that for you. No more developer effort spent on marking objects for deallocation & releasing them etc., now the the compiler takes care of this for us. The great thing about xCode is that the compiler will raise syntax errors for when you forget to declare a pointer. And whats more, the xCode  diagnostic tools (Instruments) provide some serious chops when it comes to inspecting the memory usage and allocation in your program. Something that you wont get in Rails for example.

![alt text](/path/to/img.jpg "Title")

###### Swift
Swift shares type inference just like Ruby. With the use of its keywords **let** (for constants - immutable) and **var** (for variables - mutable) developers can leverage its type inference. The string format specifier in swift is just to escape the variable within the string, much more Ruby/JS-like, without the need for Objective-C's format specifier by type. Nice!

```
// VarsConstantsStringsInterpolationObjC.m
let ide = "xCode"
var language = "Swift" println("Our \(language) example.")
```

#### Collection Types
##### Arrays
###### Ruby
Thankfully again with Ruby we can again leverage type inference. With the use of square brackets we can define an array and put objects of any type inside of it.

```
// collection_types_arrays.rb
mixed_type_array = [1, 'a', 2, 'b']
```

###### Objective-C
As is usually the case, the things that gave Objective-C a bad name are the way things *used* to be done. Take standard array declaration for instance. You had to send a message to the NSArray class passing in parameters for the objects, with nil being the last argument because we needed to be explicit about where the number of arguments ended. Variable arguments were not allowed by the compiler. But now, we can leverage Objective-C literals to sweeten up that syntax. See, the new stuff looks pretty good!

```
// CollectionTypesArrayObjC.m
old - very verbose
NSNumber *number = [NSNumber numberWithInt:12345];
NSString *letter = [[NSString alloc]initWithString:@"a"];

NSArray *array = [NSArray arrayWithObjects:number, letter, nil];

newer - terse
NSArray *mixedTypeArray = @[@1,@"a",@2,@"b"];
```

###### Swift

Swift diverges a little from Ruby and Objective-C. Swift employs explicitly typed collections. Reason? This ensures you cannot insert a value of wrong type into an array or dictionary by mistake. Apple says that being precise with content types will help developers avoid many common mistakes, and will provide more confidence when retrieving values from collections. Having said that, if the values can be inferred then they will be. So explicit declarations are not enforced but they are a heavy guideline. Here's what a Swift type safe array looks like.

#Mixed type example needed with explicit declaration

```
// CollectionTypesArraySwift.m
var languages: String[] = ["Ruby", ""Objective-C", "Swift"]

```

##### Hashes / Dictionaries

###### Ruby

Simple as usual with ruby, with the use of curly braces, we simply define our key-value pairs and we are done.

```
// collection_types_hash.rb
capital_cities = {england: "London", ireland: "Dublin", france: "paris" }
```
###### Objective-C

True to form, Objective-C is verbose. In the past you would create separate array collections of keys and the values they map to, then use **NSDictionary:dictionaryWithObjects:forKeys** to create the dictionary collection we wanted. Now, the newer, literal sytnax is much sweeter.

```
// CollectionTypesDictionaryObjC.m
old - very verbose
NSArray *countryKeys = [NSArray arrayWithObjects:@"England",@"Ireland",@"France", nil];
NSArray *capitalValues = [NSArray arrayWithObjects:@"London",@"Dublin",@"Paris", nil];

NSDictionary *capitalCities = [NSDictionary dictionaryWithObjects:countryKeys
                                       forKeys:capitalValues];

new - terse
NSDictionary *capitalCities = @{ @"England" : @"London", @"Ireland" : @"Dublin", @"France" : @"Paris" };

```

###### Swift
Again, Swift's dictionary collection is explictly typed. Here's an example:

```
// CollectionTypesDictionarySwift.m
var capitalCities: Dictionary<String, String> = ["England":"London", "Ireland":"Dublin", "France":"Paris"]
```


#### Access & Modifications

##### Arrays

###### Ruby
Ruby's intuitive syntax, means collection access and modifciation is pretty easy. For arrays, we can replace, mutate values at an index and push, pop, shift, unshift etc. into and off of an array.

```
// access_and_mods_array.rb

mixed_type_array = [1, 'a', 2, 'b']
mixed_type_array[0] = 3 // [3, 'a', 2, 'b']

mixed_type_array.pop // [3, 'a', 2]
mixed_type_array.push('b') // [3, 'a', 2, 'b']

mixed_type_array.shift // ['a', 2, 'b']
mixed_type_array.unshift(3) // [3, 'a', 2, 'b']

```

###### Objective-C

Accessing arrays with Objective-C feels familiar but modification has a slight twist. We access its contents with a reference to an index but in order to modify an array, it needs to be of class **NSMutableArray** which is a subclass of NSArray. To add objects to an array we send an instance of NSMutableArray a message of **addObject:** passing in the object to be added as a parameter.

We can also specify at what index the object should be added by passing the index to the **insertObject:atIndex:** message sent to the array. To delete objects at an index we can send the **removeObjectAtIndex** message to the mutable array instance, and if we want to be specific, we can pass an object to be used as a comparator against the contents of the array.


```
// AccessAndModsArrayObjC.m

NSMutableArray *languages = [NSMutableArray arrayWithObjects: "Ruby", "Objective-C", "Swift", nil];
NSString *ruby = []languages objectAtIndex:0];
[languages addObject:@"Go"];
[languages insertObject:"Elixir" atIndex:2];
[languages removeObjectAtIndex:0]
[languages removeObject:"Objective-C"];
```

###### Swift
Swift offers array access and modification in the vein of Ruby. Swift also allows the use of opearators such as the addition assignment operator. We can change ranges of values too with its subscript syntax.

```
// AccessAndModsArraySwift.m
var languages: String[] = ["Ruby", ""Objective-C", "Swift"]
languages.insert("Go", atIndex:0) // ["Go","Ruby", ""Objective-C", "Swift"]

languages.append("Elixir")// ["Go","Ruby", ""Objective-C", "Elixir"]

languages += "Haskell" // ["Go","Ruby", ""Objective-C", "Elixir", "Haskell"]

languages[0...1] = ["Java", "Erlang"] // ["Java", "Erlang", ""Objective-C", "Elixir", "Haskell"]
```

##### Hashes / Dictionaries

###### Ruby
Hash access in Ruby is rather simple. Simply specify the symbol or the string name of the key with square bracket notation to access its value. Send the delete message to the dictionary with the key passed as an argument and that key-value pair will be deleted from the dictionary.

We can access the key that matches a certain value by sending the **key** message to the dictionary with the parameter to match. This will return the first key that matches that value. Like its array, Ruby hashes also have a shift method but with a slight twist. Send the shift message to a Ruby hash and it will return the first key-value pair as you might expect, but the retuned object will be a two value array containing the key and the value just removed.

```
// access_and_mods_array.rb
capitalCities = {england: "London", france: "Paris", ireland: "Dublin"}
capitalCities[:england] // "London"
capitalCities.delete(:england) // {:france=>"Paris", :ireland=>"Dublin"}
capitalCities = {england: "London", france: "Paris", island: "Dublin", ireland: "Dublin"}
capitalCitites.key('Dublin') // :island
capitalCities.shift // [:england, "London"]

```

###### Objective-C
Objective-C dictionaries are the key-value pair data structure that are comparable to Ruby's Hash. Similarly to Objective-C's array's, there are mutable and immutable instances of dictionaries.

We can access objects by sending the message **objectForKey:** to any dictionary instance, passing the key parameter for which we want to find the value for. We can add/update a mutable dictionarie's key-value pairs by sending it the **setObject:forKey:** message. Need to remove an object or remove all key-value pairs in the dictionary? Use either **removeObjectForKey:** or **removeAllObjects**.

```
// AccessAndModsArraySwift.m
NSMutableDictionary *capitalCities = [NSDictionary dictionaryWithObjectsAndKeys: @"London", @"England", @"Paris", @"France", @"Dublin", @"Ireland", nil];
[capitalCities objectForKey:@"Ireland"]; // Dublin
[capitalCities addObject:@"Berlin" forKey:@"Germany"];
[capitalCities removeObjectForKey:@"Germany"];
[capitalCities removeAllObjects];

```

###### Swift

Swifts access to dictionaries is more similar to Ruby's access to hashes. Need to add an object for a key? The syntax looks just like Ruby's. Updating a key-value pair is the same too. Interestingly, Swift offers an **updateValue(forKey:)** method to set or update a value for a key, but it will also return an *optional* value that contains the old value for the key if it already existed.

So if you need to update the key-value pair but keep the value you are replacing, you can do that with **updateValue(forKey:)**. Need to remove a key-value pair from the dictionary? Just set the value to nil, or send the **removeValueForKey(key)** message to the dictionary, specifying the key.

```
// AccessAndModsArraySwift.m
var capitalCities: Dictionary<String, String> = ["England":"London", "Ireland":"Dublin", "France":"Paris"]
capitalCities["Germany"] = "Berlin"
capitalCities["Germany"] = "Bonn"

let oldCapital = capitalCities.updateValue("Berlin", forKey: "Germany")
println("The old capital of Germany was: \(oldCapital)") // "The old capital of Germany was: Bonn"
capitalCities["Ireland"] = nil
capitalCities.removeValueForKey("France")


```

As we have seen, for all the griping over Objective-C's syntax, its not *too* dissimilar to the syntax of Ruby. Swift however is much more Ruby-esque and could serve as the bait for many Ruby devs to make the jump into native iOS development. In further posts, we will explore more language similarities and delve into the implementation of each language's classes.


#### End Part 1

#### Control Flow & Enumeration

Ruby

Objective-C

Swift

#### Closures

Ruby

Objective-C

Swift


#### Creating Classes

#### Using Classes

#### Testing

#### Tooling

#### End
