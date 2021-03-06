### 0.7.0

* BREAKING CHANGE: Bodyless methods behave as if they were defined with an empty block (arity of 0, return nil)

### 0.6.4

* Remove explicit reference to RSpec::Matchers so that surrogate/rspec doesn't depend on rspec-expectations
* Add `define_setter`, `define_getter`, `define_accessor` which mimic Ruby's `attr_*` methods, but are also recorded / overridable.

### 0.6.3

* Allow mass initialization of instances with `MockClass.factory key: value`
* The `.factory` method can be turned off with `Surrogate.endow self, factory: false`
* The `.factory` method can be renamed with `Surrogate.endow self, factory: :custom_factory_method_name`
* Allow mass initialization of clones with `MockClass.clone key: value`

### 0.6.2

* Make substitutability matcher go either way (you should now do `RealClass.should substitute_for SurrogateClass` eventually, doing it in the other direction will not be supported)
* Bug fix: checks arity on invocations, even when default value is overridden
* Substitutability can check argument names
* Fix error message when there are no api methods. Used to say "Doesn't know initialize, only knows "

### 0.6.1

* bang methods map to ivars suffixed with `_b`, because you can't have a bang in an ivar name
* Add general syntax for overriding values (e.g. for use with operators) `will_overrides`
* block assertions can specify that exceptions should get raised (still shitty error messages, though) The interface mimicks RSpec's `#raise_error` matcher

### 0.6.0

* Setting an override still requires the invoking code to call with the correct signature
* Remove `api_method_names` and `api_method_for` and `invocations` from surrogates
  (might break your code if you relied on these, but they were never advertized, and no obvious reason to use them)
  Instead use the reflectors: Surrogate::SurrogateClassReflector and Surrogate::SurrogateInstanceReflector
* BREAKING CHANGE - Substitutability can check argument "types". This is turned on by default
* Initialize is no longer implicitly recorded (This might break something, but I don't think this feature was ever advertized, so hopefully people don't depend on it).
* BREAKING CHANGE - API method signatures are enforced (if meth takes 1 arg, you must pass it 1 arg)
* The name of a clone is the name of the parent suffixed with '.clone', unless the parent is anonymous (not set to a const), then the name is nil.
* Inspect messages are shorter and more helpful
* Inspect messages on class clones mimic the parents
* Remove comment about the new syntax in the Readme.  If you want to switch over, here is a shell script that should get you pretty far:

    find spec -type file |
      xargs ruby -p -i.old_syntax \
      -e 'gsub /should(_not)?(\s+)have_been_told_to/,               "was\\1\\2told_to"' \
      -e 'gsub /should(_not)?(\s+)have_been_asked_(if|for)(_its)?/, "was\\1\\2asked_\\3"' \
      -e 'gsub /should(_not)(\s+)have_been_initialized_with/,       "was\\1\\2initialized_with"' \


