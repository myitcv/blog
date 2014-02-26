---
date: 2013-03-10
layout: post
title: Rails Application Example using HTML5 and haml
location: San Francisco
author: paul
tags:
- coding
- ruby
- rails
- haml
- html5
---

This blog post borrows heavily from Daniel Kehoe's excellent [Rails Application Layout for
HTML5](http://railsapps.github.com/rails-default-application-layout.html) but is a slightly more cut down, to-the-point
version. In addition, all the code for this example [can be found on
GitHub](https://github.com/myitcv/haml-html5-example)

First begin by creating a new Rails application:

```bash
rails new haml-html5-example
cd haml-html5-example
```

Add required gems to your Gemfile:

```ruby
# Gemfile
# ...

gem 'haml'
gem 'haml-rails'
gem 'haml-contrib'
gem 'RedCloth'
gem 'bootstrap-sass'

# ...
```

Let's ensure all the required gems are installed and in place:

```bash
bundle update
```

Now we need to flip the original default application layout template from erb to haml:

```bash
mv app/views/layouts/application.html.erb app/views/layouts/application.html.haml
```

Edit the default layout, replacing the contents (which will be left over from the erb template) to instead read:

```haml
-# app/views/layouts/application.html.haml

-# Jam in a flash[:notice] so that we can see the flash rendering in effect
- flash[:notice] = "This is a test"
!!!
%html
  %head
    %meta{:name => "viewport", :content => "width=device-width, initial-scale=1, maximum-scale=1"}
    %title= content_for?(:title) ? yield(:title) : "App_Name"
    %meta{:content => content_for?(:description) ? yield(:description) : "App_Name", :name => "description"}
    = stylesheet_link_tag :application, :media => "all"
    = javascript_include_tag :application
    = csrf_meta_tags
    = yield(:head)
  %body
    %header.navbar.navbar-fixed-top.navbar-inverse
      %nav.navbar-inner
        .container
	  %a.brand{:href => "/"} Our First HTML5 haml example
    #main{:role => "main"}
      .container
        .content
          .row
            .span12
              = render 'layouts/messages'
              = yield
          %footer
```

Now define a messages partial for the rendering of [flash messages](http://guides.rubyonrails.org/action_controller_overview.html#the-flash) :

```haml
-# app/views/layouts/_messages.html.haml
- flash.each do |name, msg|
  - if msg.is_a?(String)
    %div{:class => "alert alert-#{name == :notice ? "success" : "error"}"}
      %a.close{"data-dismiss" => "alert"} ×
      = content_tag :div, msg, :id => "flash_#{name}"
```

Include the Twitter Bootstrap Javascript files (this is incidentally why we need the `bootstrap-sass` gem):

```javascript
/* app/assets/javascripts/application.js */

//= require jquery
//= require jquery_ujs
//= require bootstrap
//= require_tree .
```

To keep things clean, we import the Twitter bootstrap CSS files and include any overrides in a new file (which is picked up by the `require_tree` directive above):

```scss
/* app/assets/stylesheets/bootstrap_and_overrides.css.scss */

@import "bootstrap";
body { padding-top: 60px; }
@import "bootstrap-responsive";
```

The padding override allows us to accommodate the [Bootstrap black nav bar](http://twitter.github.com/bootstrap/examples/hero.html) .

Daniel then goes on to give a neat example of how to provide an override that gives your content a nice grey surround:

```scss
/* app/assets/stylesheets/bootstrap_and_overrides.css.scss */

@import "bootstrap";
body { padding-top: 60px; }
@import "bootstrap-responsive";

.content {
  background-color: #eee;
  padding: 20px;
  margin: 0 -20px; /* negative indent the amount of the padding to maintain the grid system */
  @include border-radius(0 0 6px 6px);
  @include box-shadow(0 1px 2px rgba(0,0,0,0.15));
}
```

Just to tidy things up, rename the default `application.css` file:

```bash
mv app/assets/stylesheets/application.css app/assets/stylesheets/application.css.scss
```

Now let's create a really simple view that exercises this setup (as well as removing the default `index.html`):

```bash
rm public/index.html
rails generate controller home index
```

Set the default route to point to this new view:

```ruby
# config/routes.rb

HamlHtml5Example::Application.routes.draw do
  get "home/index"
  root :to => 'home#index'
end
```

We should now be ready to fire up our application and see the results:

```bash
rails server
```

The results should look something like this:

![](/images/2013-03-10-html5-haml-example.png)

Now let's get a bit more creative and override the default content in our `home#index` view (this is where we need the `haml-contrib` and `RedCloth` gem files):

```haml
# app/views/home/index.html.haml

%h1 Heading 1
%p A paragraph

- if true
  %p True

%ul
  - [1,2,3].each do |element|
    %li=element

:javascript
  $(document).ready(function() {
    alert("#{flash[:notice]}");
  });

:textile
  I prefer to write my documents using **textile**
```

Much better.

All the code for this example [can be found on GitHub](https://github.com/myitcv/haml-html5-example)