---
title: Ruby on the Frontend? Choose Your Weapon
subtitle: We all know that Ruby is a great language to use for the backend of your web application, but did you know you can write Ruby code for the frontend as well?
categories:
- Frontend Development
author: jared
image: "/images/posts/muscle-car.jpg"
image_hero: "/images/posts/muscle-car.jpg"
image_credit:
  url: https://unsplash.com/photos/BggnruUUlXI
  label: Meritt Thomas on Unsplash
published: true
---

We all know that Ruby is a great language to use for the backend of your web application, but did you know you can write Ruby code for the frontend as well?

Not only that, but there are _two_ available options to choose from when looking to “transpile” from Ruby to Javascript. These are:

* [Ruby2JS](https://github.com/rubys/ruby2js)
* [Opal](https://www.opalrb.com)

Let’s take a quick peek at each one and see what might be right for your project.

### Ruby2JS

My personal favorite, [Ruby2JS](https://github.com/rubys/ruby2js) was created by Sam Ruby (yep, that's his name), and it is intended to convert Ruby-like syntax to Javascript as cleanly and “natively” as possible. This means that (most of the time) you’ll get a line-by-line, 1:1 correlation between your source code and the JS output. For example:

```ruby
class MyClass
  def my_method(str)
    ret = "Nice #{str} you got there!"
    ret.upcase()
  end
end
```

will get converted to:

```js
class MyClass {
  myMethod(str) {
    let ret = `Nice ${str} you got there!`;
    return ret.toUpperCase()
  }
}
```

There’s actually a lot going on here so let me unpack it for you:

* Depending on how you configure Ruby2JS, you can convert classes to old-school JS functions/constructors, or you can use modern ES6+ classes like in the example here (which I recommend).
* Ruby2JS provides “filters” which you can apply selectively to your code to enable new functionality. In this example, the `camelCase` filter automatically converts typical Ruby snake\_case to camelCase as is common in Javascript. The `functions` filter automatically converts many popular Ruby methods into JS counterparts (so `upcase` becomes `toUpperCase`). And the `return` filter automatically add a return to the end of a method just like how Ruby works.
* String interpolation in Ruby magically become valid ES6+ string interpolation, and it even works with squiggly heredocs!

How do you get started using Ruby2JS? It’s pretty simple: if you’re using a framework with Webpack support (Rails, [Bridgetown](https://www.bridgetownrb.com)), you can add the [rb2js-loader plugin](https://github.com/whitefusionhq/rb2js-loader) along with the ruby2js gem, write some frontend files with a `.js.rb` extension, and import those right into your JS bundle. It even supports source maps right out of the box so if you have any errors, you can see the original Ruby source code right in your browser’s dev inspector!

**Full disclosure:** I recently joined the Ruby2JS team and built the Webpack loader, so let me know if you run into any issues and I’ll be glad to help!

### Opal

The [Opal](https://www.opalrb.com) project was founded by Adam Beynon in 2012 with the ambitious goal of implementing a nearly-full-featured Ruby runtime in Javascript, and since then it has grown to support an amazing number of projects, frameworks, and use cases.

There are plenty of scenarios where you can take pretty sophisticated Ruby code, port it over to Opal as-is, and it just compiles and runs either via Node or in the browser which is pretty impressive.

Because Opal implements a Ruby runtime in Javascript, it adds many additional methods to native JS objects (strings, integers, etc.) using a `$` prefix for use within Opal code. Classes are also implemented via primitives defined within Opal’s runtime layer. All this means that the final JS output can sometimes look a little closer to bytecode than traditional JS scripts.

For instance, the above example compiled via Opal would result in:

```js
/* Generated by Opal 1.0.3 */
(function(Opal) {
  var self = Opal.top, $nesting = [], nil = Opal.nil, $$$ = Opal.const_get_qualified, $$ = Opal.const_get_relative, $breaker = Opal.breaker, $slice = Opal.slice, $klass = Opal.klass;

  Opal.add_stubs(['$upcase']);
  return (function($base, $super, $parent_nesting) {
    var self = $klass($base, $super, 'MyClass');

    var $nesting = [self].concat($parent_nesting), $MyClass_my_method$1;

    return (Opal.def(self, '$my_method', $MyClass_my_method$1 = function $$my_method(str) {
      var self = this, ret = nil;

      
      ret = "" + "Nice " + (str) + " you got there!";
      return ret.$upcase();
    }, $MyClass_my_method$1.$$arity = 1), nil) && 'my_method'
  })($nesting[0], null, $nesting)
})(Opal);
```

Thankfully, Opal too has support for source maps so you rarely need to look at anything like the above in day-to-day development—instead, your errors and debug output will reference clean Ruby source code in the dev inspector.

One of the more well-known frameworks using Opal is [Hyperstack](https://hyperstack.org). Built on top of both Opal and React, Hyperstack lets you write “isomorphic” code that can run on both the server and the client, and you can reason about your web app using a well-defined component architecture and Ruby DSL.

### Conclusion

As you look at the requirements for your project, you can decide whether Ruby2JS or Opal might suit your needs.

* If you use Webpack and already have a lot of JS code or libraries you need to interoperate with, [Ruby2JS](https://github.com/rubys/ruby2js) is a capable and lightweight solution which integrates easily into your build pipeline.
* If you’re starting from scratch and want all the power of a full Ruby runtime as well as opportunities to write isomorphic Ruby code, [Opal](https://www.opalrb.com) might be just what the doctor ordered.

Regardless of which you choose, it’s exciting to know that we can apply our Ruby knowledge to the frontend as well as the backend for web applications large and small. It’s a great day to be a Rubyist. {%= ruby_gem %}