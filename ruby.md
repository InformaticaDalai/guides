# Ruby

Much of this was taken from:

* [The Ruby Style Guide](https://github.com/bbatsov/ruby-style-guide#classes--modules)
* [Thoughtbot](https://github.com/thoughtbot/guides)
* [Github](https://github.com/styleguide/ruby)

## Coding layout

* Use two **spaces** per indentation level (aka soft tabs). No hard tabs.

  ```Ruby
  # bad - four spaces
  def some_method
      do_something
  end

  # good
  def some_method
    do_something
  end
  ```

* Use `UTF-8` as the source file encoding

* Never leave trailing whitespace.

* End each file with a blank newline.

* Use spaces around operators, after commas, colons and semicolons, around { and before }.

    ```Ruby
    sum = 1 + 2
    a, b = 1, 2
    1 > 2 ? true : false; puts "Hi"
    [1, 2, 3].each { |e| puts e }
    ```

* No space after !

    ```Ruby
    # bad
    ! something

    # good
    !something
    ```

* Indent `when` as deep as `case`

    ```Ruby
    case
    when song.name == 'Misty'
      puts 'Not again!'
    when song.duration > 120
     puts 'Too long!'
    when Time.now.hour > 21
     puts "It's too late"
    else
     song.play
    end

    kind = case year
           when 1850..1889 then 'Blues'
           when 1890..1909 then 'Ragtime'
           when 1910..1929 then 'New Orleans Jazz'
           when 1930..1939 then 'Swing'
           when 1940..1950 then 'Bebop'
           else 'Jazz'
           end
    ```

* Add a new line after conditionals, blocks, case statements, etc.


    ```Ruby
    if robot.is_awesome?
      send_robot_present
    end

    robot.add_trait(:human_like_intelligence)
    ```

* Use spaces around the = operator when assigning default values to method parameters

    ```Ruby
    # bad
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # do something...
    end

    # good
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # do something...
    end
    ```

* Add underscores to large numeric literals to improve their readability. 

    ```Ruby
    # bad - how many 0s are there?
    num = 1000000

    # good - much easier to parse for the human brain
    num = 1_000_000
    ```

## Line Length

* Keep each line of code to a readable length. Unless you have a reason to, keep lines to fewer than 100 characters. Keeping code visually grouped together (as a 100-character line limit enforces) makes it easier to understand. For example, you don't have to scroll back and forth on one line to see what's going on, you can view it all together.


## Documentation

* Use TomDoc to the best of your ability. It's pretty sweet:

    ```Ruby
    # Public: Duplicate some text an arbitrary number of times.
    #
    # text  - The String to be duplicated.
    # count - The Integer number of times to duplicate the text.
    #
    # Examples
    #
    #   multiplex("Tom", 4)
    #   # => "TomTomTomTom"
    #
    # Returns the duplicated String.
    def multiplex(text, count)
      text * count
    end
    ```

### Punctuation, spelling and grammar

* Pay attention to punctuation, spelling, and grammar; it is easier to read well-written comments than badly written ones.

* Comments should be as readable as narrative text, with proper capitalization and punctuation. In many cases, complete sentences are more readable than sentence fragments. Shorter comments, such as comments at the end of a line of code, can sometimes be less formal, but you should be consistent with your style.

* Although it can be frustrating to have a code reviewer point out that you are using a comma when you should be using a semicolon, it is very important that source code maintain a high level of clarity and readability. Proper punctuation, spelling, and grammar help with that goal.


### Commented-out code

* Never **ever** leave commented-out code in our codebase.

## Syntax

* Never use `for`, unless you know exactly why. Most of the time iterators should be used instead. `for` is implemented in terms of each (so you're adding a level of indirection), but with a twist, `for` doesn't introduce a new scope (unlike each) and variables defined in its block will be visible outside it.

    ```Ruby
    arr = [1, 2, 3]

    # bad
    for elem in arr do
      puts elem
    end

    # good
    arr.each { |elem| puts elem }
    ```

* Never use then for multi-line if/unless.

    ```Ruby
    # bad
    if some_condition then
      # body omitted
    end

    # good
    if some_condition
      # body omitted
    end
    ```

* Avoid the ternary operator (?:) except in cases where all expressions are extremely trivial. However, do use the ternary operator(?:) over if/then/else/end constructs for single line conditionals.

    ```Ruby
    # bad
    result = if some_condition then something else something_else end

    # good
    result = some_condition ? something : something_else
    ```

* Favor modifier if/unless usage when you have a single-line body.

    ```Ruby
    # bad
    if some_condition
     do_something
    end

    # good
    do_something if some_condition
    ```

* Never use unless with else. Rewrite these with the positive case first.

    ```Ruby
    # bad
    unless success?
      puts "failure"
    else
      puts "success"
    end

    # good
    if success?
     puts "success"
    else
      puts "failure"
    end
    ```

* Don't use parentheses around the condition of an if/unless/while.

    ```Ruby
    # bad
    if (x > 10)
      # body omitted
    end

    # good
    if x > 10
     # body omitted
    end
    ```

* Avoid return where not required.

    ```Ruby
    # bad
    def some_method(some_arr)
      return some_arr.size
    end

    # good
    def some_method(some_arr)
      some_arr.size
    end
    ```

* Use ||= freely to initialize variables.
    
    ```Ruby
    # set name to Bozhidar, only if it's nil or false
    name ||= "Bozhidar"
    ```

### Naming

* Use snake_case for methods and variables.

* Use CamelCase for classes and modules. (Keep acronyms like HTTP, RFC, XML uppercase.)

* Use SCREAMING_SNAKE_CASE for other constants.

* The names of predicate methods (methods that return a boolean value) should end in a question mark. (i.e. Array#empty?).

* The names of potentially "dangerous" methods (i.e. methods that modify self or the arguments, exit!, etc.) should end with an exclamation mark. Bang methods should only exist if a non-bang method exists. (More on this).

## Methods

* Use parentheses for a method call if the method returns a value.

    ```Ruby
    # bad
    @current_user = User.find_by_id 1964192

    # good
    @current_user = User.find_by_id(1964192)
    ```

* If the first argument to the method uses parentheses.

    ```Ruby
    # bad
    put! (x + y) % len, value

    # good
    put!((x + y) % len, value)
    ```

## Classes


* Use a consistent structure in your class definitions. 

    ```Ruby
    class Person
      # extend and include go first
      extend SomeModule
      include AnotherModule

      # inner classes
      CustomErrorKlass = Class.new(StandardError)

      # constants are next
      SOME_CONSTANT = 20

      # afterwards we have attribute macros
      attr_reader :name

      # followed by other macros (if any)
      validates :name

      # public class methods are next in line
      def self.some_method
      end

      # initialization goes between class methods and other instance methods
      def initialize
      end

      # followed by other public instance methods
      def some_method
      end

      # protected and private methods are grouped near the end
      protected

      def some_protected_method
      end

      private

      def some_private_method
      end
    end
    ```

* Consider using Struct.new, which defines the trivial accessors, constructor and comparison operators for you.
 
    ```Ruby
    # good
    class Person
      attr_accessor :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end

    # better
    Person = Struct.new(:first_name, :last_name) do
    end
    ```

* Indent the public, protected, and private methods as much as the method definitions they apply to. Leave one blank line above the visibility modifier and one blank line below in order to emphasize that it applies to all methods below it. 

    ```Ruby
    class SomeClass
      def public_method
        # ...
      end

      private

      def private_method
        # ...
      end

      def another_private_method
        # ...
      end
    end
    ```

## Collections

* Prefer `%w` to the literal array syntax when you need an array of strings.
   
    ```Ruby
    # bad
    STATES = ["draft", "open", "closed"]

    # good
    STATES = %w(draft open closed)
    ```

* Use Set instead of Array when dealing with unique elements. Set implements a collection of unordered values with no duplicates. This is a hybrid of Array's intuitive inter-operation facilities and Hash's fast lookup.

* Use symbols instead of strings as hash keys.

    ```Ruby
    # bad
    hash = { "one" => 1, "two" => 2, "three" => 3 }

    # good
    hash = { :one => 1, :two => 2, :three => 3 }
    
    # better
    hash = { one: 1, two: 2, three: 3}
    ```

## Strings

* Prefer string interpolation instead of string concatenation:
    
    ```Ruby
    # bad
    email_with_name = user.name + " <" + user.email + ">"

    # good
    email_with_name = "#{user.name} <#{user.email}>"
    ```

* Use double-quoted strings. Interpolation and escaped characters will always work without a delimiter change, and ' is a lot more common than " in string literals.`

    ```Ruby
    # bad
    name = 'Bozhidar'

    # good
    name = "Bozhidar"
    ```
