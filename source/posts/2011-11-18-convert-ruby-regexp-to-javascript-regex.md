---
layout: post
title: Convert Ruby Regexp to JavaScript RegExp
comments: true
author: Brian Cardarella
twitter: bcardarella
github: bcardarella
summary: A simple extraction from ClientSideValidations
category: ruby
social: true
---

This has a very limited use case, but I needed it for
[ClientSideValidations](http://github.com/bcardarella/client_side_validations). It took a while
to track down some of the possible conversion issues, I figure someone
else might find this useful.

```ruby
class Regexp
  def to_javascript
    Regexp.new(inspect.sub('\\A','^').sub('\\Z','$').sub('\\z','$').sub(/^\//,'').sub(/\/[a-z]*$/,'').gsub(/\(\?#.+\)/, '').gsub(/\(\?-\w+:/,'('), self.options).inspect
  end
end
```

When you render it to the client simply instantiate a new `RegExp`
object with the resulting string:

```javascript
new RegExp(regexpStringFromRuby);
```

If there are any edge-cases that won't convert cleanly please report
them in the comments.

See how it is being used in `ClientSideValidations` [to_json](https://github.com/bcardarella/client_side_validations/blob/master/lib/client_side_validations/core_ext/regexp.rb)
[tests cases](https://github.com/bcardarella/client_side_validations/blob/master/test/core_ext/cases/test_core_ext.rb)
