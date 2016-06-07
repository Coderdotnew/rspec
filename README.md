# rspec
**Rspec** is a Ruby testing library that is used to for **Test Driven Development** or **TDD**. Think of a library as an extension of a language that uses the same syntax and structure, but has some added keywords and functionality. The added functionality of Rspec is that it can test our Ruby code to ensure a program is running smoothly without syntax or logic error and returns the expected values.  
![1](http://i.imgur.com/oQKDiVO.gif)

Rspec is a Ruby **gem** that is downloaded for most of the code challenges. A Ruby gem, as you will learn, is pre-packaged code that can be downloaded and used in your program. 

#### Note: Each Ruby Rspec file used in this course will be fully written and linked. Your job is to focus on writing top-notch Ruby code as well as being able to read and understand the Rspec file to ensure your code will pass the tests. You *do not* need to write the Rspec files, but reading an Rspec file is critical to your success.

# why_TDD?
Think back to your last essay your wrote for your English class. Did you create a detailed outline before writing your essay? Did you proofread it? Did you run a spellcheck on it? Did you have family member or friend read it over for you? If you didn't, you likely received your test back with some red marks from your teacher. These red marks might indicate spelling errors, grammar errors, run-on sentences, incoherent logic or structure, and other mistakes made.  

Think of Test-Driven Development as way for the programmer to map out the program, descriptions of how the program should run, and most importantly, what return values should be expected. Let's checkout how Rspec works.

# rspec_syntax
An Rspec file is connect to the Ruby file it is running the tests for. The files will already be linked for you, but here's what this looks like.  

Let's imagine we are working in a Ruby file, `ruby.rb`. By convention, Rspec requires the testing file to be titled the exact same thing, with `_spec` added to indicate it is an Rspec file. Therefore, our two files are:  
1. `ruby.rb`
2. `ruby_spec.rb`  
Notice, the file extension is still `.rb` as this is written in Ruby  

(Just to quiz you, what is our Rspec file called if the Ruby file is saved as `my_program.rb`?  
That's right... `my_program_spec.rb`)

Each Rspec test needs to be organized in a directory as follows:  
```bash
sinatra_app_directory
├── directory_name
    ├── spec # this is the rspec directory
        └── ruby_spec.rb # this is your rspec file, do not alter this file
        └── spec_helper.rb # do not alter this file
    └── ruby.rb # our ruby file
```
- Just to recap this structure:
  - The `directory_name` will be the name of the code challenge directory. This directory will contain the `spec` directory as well as the Ruby file.
  - The `ruby_spec.rb` is the rspec file. This contains the tests. You will read this thoroughly but **do not alter this code**
  - The `spec_helper.rb` file is auto-generated once the rspec gem is installed. This connects to the rspec library and configures our code for us. **Do not alter this code, either**  

Let's check out the syntax, finally...

```ruby
# ruby_spec.rb
require_relative './spec_helper'
require_relative '../ruby.rb'
```
- Each Rspec file will `require_relative`, meaning it is linking to another file in the directory
  - We need to link to `spec_helper.rb` because this configures our rspec file
  - We need to link to `ruby.rb` as this is the Ruby code the Rspec file is testing  

```ruby
# ruby_spec.rb
require_relative './spec_helper'
require_relative '../ruby.rb'

describe "#hello" do

end
```
- **describe**: The `describe` keyword is used to identify a class or a method that will perform a function. In this case, we test is describing a method, called `hello`
  - `hello` can be identified as a method as it is preceded by a `#` which indicated a method name will follow
- **do**: The `do` keyword is recognizable from enumeration and loops. This requires an `end` keyword and is proceded by a description of the test  

# description
Once the method name has been defined, the next line is reserved for describing the desired function of the method in English terms (not Ruby).  

We want the `hello` method to `print` and `return` an expected string.  
```ruby
# ruby_spec.rb
require_relative './spec_helper'
require_relative '../ruby.rb'

describe "#hello" do
    it "prints the expected string to the screen" do
    
    end
end
```
- Our test is slightly more descriptive now. 
- Our last step is to determine what our expected return values are

# rspec_and_puts
In our the first 3 classes, there are tests that require an printed output to the screen. This requires the `puts` or `print` keyword.  

The tricky thing is that using `puts` or `print` leaves us with a reutrn value of `nil`, which isn't helpful to an Rspec test that specifically tests for a return value.  

We need to use specific syntax for this instance.
```ruby
# ruby_spec.rb
require_relative './spec_helper'
require_relative '../ruby.rb'

describe "#hello" do
    it "prints the expected string to the screen" do
        expect($stdout).to receive(:puts).with("Hello, Barack Obama!") 
        hello 
    end
end
```
- Don't worry too much about `$stdout` (standard output). This is just a placeholder acknoledging there will be an output rather than a return value.
- To make this even clearer, `:puts` is explicitly included in the Rspec test as well as the expected string: `"Hello, Barack Obama!"`
- Laslty, and perhaps most importantly, Rspec *calls* the method for us. We do *not* need to call the method. 
- `hello` is explicilty called on the next line.

# rspec_and_return
A majority of our Rspec tests will tests for specific return values (i.e. not using `puts`). This syntax is a bit more straightforward. 
```ruby
# ruby_spec.rb
require_relative './spec_helper'
require_relative '../ruby.rb'

describe "#hello" do
    it "prints the expected string to the screen" do
        expect(hello).to eq("Hello, Barack Obama!")
    end
end
``` 
- If this is read across it might sound something like: The Rspec test expects the `hello` method to **eq**uate to a return value of `"Hello, Barack Obama!"`
- In the case of explicit returns, the method is called inside the parentheses after the `expect` keyword

# running_rspec_tests
Great! Now we have an Rspec file that expects the following from our Ruby file:
1. A method titled `hello`
2. The `hello` method is expected to print the string `"Hello, Barack Obama!"` to the screen using `puts`
3. The `hello` method is expected to return the string value `"Hello, Barack Obama!"`  

Our Ruby file will resemble the following:  
```ruby
def hello
  name = "Barack Obama"
  puts "Hello, #{name}!"
end
```
- To run the Rspec test, we need to:
  1. Be in the root directory of the current challenge
  2. Type the command `rspec` into your Terminal

- The following will be displayed once the test is run:
```bash
#ruby
  prints the expected string to the screen
Hello, Barack Obama!
  returns the expected string (FAILED - 1)

Failures:

  1) #ruby returns the expected string
     Failure/Error: expect(hello).to eq("Hello, Barack Obama!")
     
       expected: "Hello, Barack Obama!"
            got: nil
     
       (compared using ==)
     # ./spec/ruby_spec.rb:11:in `block (2 levels) in <top (required)>'

Finished in 0.04297 seconds (files took 0.1844 seconds to load)
2 examples, 1 failure

```
- This shows we passed one test (printing the expected string to the screen)  
- However, we failed the second test.  
  - Rspec tells us it `expected: "Hello, Barack Obama!"` but `got: nil`

We can easily fix this by adding another line the will also return the expected value

```ruby
def hello
  name = "Barack Obama"
  puts "Hello, #{name}!"
  return "Hello, #{name}!"
end
```
Now let's run the test...
```bash
#ruby
  prints the expected string to the screen
Hello, Barack Obama!
  returns the expected string

Finished in 0.0133 seconds (files took 0.17814 seconds to load)
2 examples, 0 failures
```
Now we can see we passed both tests!

# rspec_and_arguments
Lastly, let's review what it looks like when our methods have arguments. This is going to be very similar to our previous example without arguments.

Because our methods are explicitly called in the Rspec file, we need to inform the test that we are also expecting an argument.
```ruby
require_relative './spec_helper'
require_relative '../ruby.rb'

describe "#ruby" do
  it 'prints the expected string to the screen' do
    expect($stdout).to receive(:puts).with("Hello, Barack Obama!") 
    hello("Barack Obama")
  end

  it 'returns the expected string' do
    expect(hello("Barack Obama")).to eq("Hello, Barack Obama!")
  end
end
```
- This is almost exactly the same, with the exception of parameters being passed in where the arguments were defined.
- Notice the actual value must be passed in in the Rspec test
- This is the same as calling the method yourself within the Ruby file (but now the Rspec file is calling the method for us)

Now we need to slightly rearrange our Ruby code...
```ruby
def hello(name)
  puts "Hello, #{name}!"
  return "Hello, #{name}!"
end
```
- When `rspec` is run in the Terminal both our tests pass for the `hello` method now that
- Note: If we do not define an argument in our Ruby file, we will get an error stating Rspec expected 1 argument but the Ruby file defined 0 arguments  

# documentation
Rspec documentation: [RSpec](https://relishapp.com/rspec) 

RSpec will become easier to read and naviagte as you begin completing challenges. There are many other tests RSpec can run which you will run into as the challenges increase in complexity.

#### TEST AWAY!  
![2](http://i.imgur.com/D9nukC2.gif)


