# Sequenced

Sequenced is a simple Rails 3 engine that generates scoped sequential 
IDs for ActiveRecord models. The sequential ID generated by Sequenced does not 
take the place of the normal primary key, but rather adds another way to retrieve 
the object within the scope of another column.

## Purpose

It's generally a bad practice to expose your database primary keys to the world 
in your URLs. However, often times it is appropriate to number objects in
sequence in the context of another object.

For example, given a Question model that has many Answers, it makes sense
to number answers sequentially within the scope of the parent question. 
You can achieve this with Sequenced in one line of code:

```ruby
class Question < ActiveRecord::Base
  has_many :answers
end

class Answer < ActiveRecord::Base
  belongs_to :question
  acts_as_sequenced :scope => :question_id
end
```

## Installation

First, add the gem to your Gemfile:

```    
gem 'sequenced'
```
    
Then, install and run migrations:

```
rails generate sequenced
rake db:migrate
```

## Usage

To add a sequential ID to a model, first add an integer column called 
`sequential_id` to the model (or you many name the column anything you
like and override the default). For example:

```
rails generate migration add_sequential_id_to_answers sequential_id:integer
rake db:migrate
```

Then, call the `acts_as_sequenced` macro in your model class:

```ruby
class Answer < ActiveRecord::Base
  belongs_to :question
  acts_as_sequenced :scope => :question_id
end
```

The `:scope` option can be any attribute, but will typically be the foreign
key of an associated parent object.

## Configuration

### Overriding the default sequential ID column

By default, Sequenced uses the `sequential_id` column and assumes it already exists. 
If you wish to store the sequential ID in different integer column, simply specify the 
column name with the `:column` option:

```ruby
acts_as_sequenced :scope => :question_id, :column => :custom_sequential_id
```

### Starting the sequence at a specific number

By default, Sequenced begins sequences with 1. To start at a different integer,
simply set the `:start_at` option:

```ruby
acts_as_sequenced :scope => :question_id, :start_at => 1000
```

## License

Copyright &copy; 2012 Derrick Reimer

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.