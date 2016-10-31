1 Overview
====

### Why use validations

* Database level validation. Database-level validations can safely handle some things (such as **uniqueness in heavily-used tables**) that can be difficult to implement otherwise.
* Client-side validation
* Controller level
* Model level. This is *Active Recorad*'s way

### When does validation happen?

Creating and saving a new record will send an SQL `INSERT` operation to the database. Updating an existing record will send an SQL `UPDATE` operation instead. **Validations** are typically run before these commands are sent to the database.

### `valid?` and `invalid?`

You can also run these validations on your own. `valid?` triggers your validations and returns `true` if no errors were found in the object, and `false` otherwise.

```ruby
class Person < ApplicationRecord
  validates :name, presence: true
end

Person.create(name: "Foo Bar").valid? #=> true
Person.create(name: nil).valid? #=> false
```

**An object instantiated with `new` will not report errors if it's technically invalid**, because validations are automatically run only when the object is saved.

```ruby
class Person < ApplicationRecord
  validates :name, presence: true
end

p = Person.new
# => #<Person id: nil, created_at: nil, updated_at: nil, name: nil>
p.errors.messages
# => {}

p.valid?
# => false
p.errors.messages
# => {:name=>["can't be blank"]}

p = Person.create
# => #<Person id: nil, created_at: nil, updated_at: nil, name: nil>
p.errors.messages
# => {:name=>["can't be blank"]}

p.save
# => false

p.save!
# => ActiveRecord::RecordInvalid: Validation failed: Name can't be blank

Person.create!
# = > ActiveRecord::RecordInvalid: Validation failed: Name can't be blank
```

2 Validation Helpers
=====

### 2.1 `acceptance`

```ruby
class Person < ApplicationRecord
  validates :term_of_service, acceptance: true
end
```

This check is performed only if `terms_of_service` is not `nil`.

```ruby

```
