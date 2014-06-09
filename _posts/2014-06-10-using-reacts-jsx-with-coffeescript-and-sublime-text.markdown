---
layout: post
title:  "Using React's JSX with CoffeeScript and Sublime Text"
tags: coffeescript react sublime
comments: true
---

Like React and JSX but also prefer CoffeeScript to JavaScript? Until recently, you were forced to write your React component renders by [shelling out to JS with backticks][blog1] or [desugaring React.DOM to create lightweight DSL][blog2].

Luckily for us wanting the real deal, [James Friend][jsdf] has created [CoffeeScript transformer for JSX][coffee-react-transform] which allows you to write that nicely formatted JSX also in your .coffee files.

[coffee-react-transform] can be used through [CLI][cli] or [API][api]. If you want CoffeeScript executable that understands CJSX, you can use [coffee-react] from the same author.

## Requiring CJSXified .coffee files in node

If you want to use the vanilla coffee executable to require CJSX enhanced files, you can use [node-cjsx] module, which transparently compiles CJSX with [coffee-react-transform] before second compilation step with regular coffee binary. Just add `require('node-cjsx').transform()` to your app and you're good to go.

## Sublime Text

If you use [Sublime Text][sublime-text], there's a way to make working with CJSX much more enjoyable.

First, you can install [sublime-react] which just had [CJSX support committed][sublime-react-cjsx] in by [Tim Griesser][tgriesser]. It provides syntax highlighting and [autocompletion snippets][snippets] for CJSX.

Second, you can change [Better CoffeeScript][better-coffeescript] plugin to use previously mentioned [coffee-react] as its coffee executable. Easy way to do that is to create symbolic link from your cjsx binary (e.g. _~/.nvm/v0.10.28/bin/cjsx_) to _~/bin/coffee_ and setting _~/bin_ as __`"binDir"`__ in _CoffeeScript.sublime-settings_. Also remember to change __`"envPATH"__ in that same settings file to a directory containing node executable (e.g. _~/nvm/v0.10.28/bin_).

__NOTE:__ _Replace ~ with your home dir, so e.g. ~/bin becomes /Users/hoppula/bin, Sublime does not seem to understand ~/ in config files_.

Now you'll get a nice preview of compiled JavaScript when you press `ALT+SHIFT+D` while editing your component's .coffee file.

#### CJSX to JavaScript conversion example

##### CoffeeScript source
{% highlight coffeescript %}
# @cjsx React.DOM
React = require 'react'

Team = React.createClass
  render: ->
    <div className="team">
      <div>{@props.team.name}</div>
      <div>{@props.team.city}</div>
    </div>

module.exports = Team
{% endhighlight %}

##### Compiled JavaScript
{% highlight javascript %}
var React, Team;

React = require('react');

Team = React.createClass({
  render: function() {
    return React.DOM.div({
      "className": "team"
    }, React.DOM.div(null, this.props.team.name),
    React.DOM.div(null, this.props.team.city));
  }
});

module.exports = Team;
{% endhighlight %}

## Browserify

[James Friend][jsdf] has also written [coffee-reactify] browserify plugin, so you can easily use that same view code when rendering on server and in browser. I'll write another blog post later on about developing isomorphic CoffeeScript apps with React and [Exoskeleton][exojs].

[jsdf]: https://github.com/jsdf
[tgriesser]: https://github.com/tgriesser
[coffee-react-transform]: https://github.com/jsdf/coffee-react-transform
[coffee-react]: https://github.com/jsdf/coffee-react
[coffee-reactify]: https://github.com/jsdf/coffee-reactify
[sublime-text]: http://www.sublimetext.com/
[sublime-react]: https://github.com/reactjs/sublime-react
[sublime-react-cjsx]: https://github.com/reactjs/sublime-react/pull/9
[node-cjsx]: https://github.com/SimonDegraeve/node-cjsx
[better-coffeescript]: https://github.com/aponxi/sublime-better-coffeescript
[blog1]: http://www.rigelgroupllc.com/blog/2013/11/19/using-reactjs-with-coffeescript/
[blog2]: http://blog.vjeux.com/2013/javascript/react-coffeescript.html
[cli]: https://github.com/jsdf/coffee-react-transform#cli
[api]: https://github.com/jsdf/coffee-react-transform#api
[snippets]: https://github.com/reactjs/sublime-react#documentation-of-available-snippets-jsx
[exojs]: http://exosjs.com/