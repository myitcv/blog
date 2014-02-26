---
date: 2013-03-04
layout: post
title: A simple User email confirmation model
location: San Francisco
author: paul
tags:
- coding
- ruby
- rails
- aasm
- html5
- haml
- bootstrap
- bootstrap_forms
- cancan
- rolify
- omniauth
- oauth2
- figaro
- observer
- draper
---

This example builds a simple email verification system based principally on an [`aasm`](https://github.com/aasm/aasm)
state machine.

It uses the following gems/concepts:

| `gem` / Concept | Usage |
|-------------------|-------|
| [`haml`](https://github.com/haml/haml) , [`haml-rails`](https://github.com/indirect/haml-rails) , [`haml-contrib`](https://github.com/haml/haml-contrib) , [`RedCloth`](https://github.com/jgarber/redcloth) | Layout and markup for rendering. Along with `bootstrap-sass` this builds on a [previous example for building simple HTML5 applications](/2013/03/10/rails-application-example-using-html5-and-haml.html) |
| [`bootstrap-sass`](https://github.com/thomas-mcdonald/bootstrap-sass) | Twitter-based styling. See above comment |
| [`aasm`](https://github.com/aasm/aasm) | Used to define the state machine behind our user email confirmation system. The state machine is defined with our `User` model |
| [`rolify`](https://github.com/EppO/rolify) , [`cancan`](https://github.com/ryanb/cancan) | Used in combination to define an admin role but also the actions individual users can perform on `User` instances |
| [`omniauth-google-oauth2`](https://github.com/zquestz/omniauth-google-oauth2) | We authenticate the entire application using `omniauth`, specifically `omniauth-google-oauth2`. This builds on a [previous example using this gem](/2013/02/19/omniauth-google-oauth2-example.html) |
| [`figaro`](https://github.com/laserlemon/figaro) | Used for our environment management. Again, build on a [previous example](/2013/03/10/using-figaro-and-bash-for-a-smoother-development-config-environment.html) |
| [`bootstrap_forms`](https://github.com/sethvargo/bootstrap_forms) | Simple but useful gem to cut down on the amount of layout coding when using `bootstrap-sass` and `form_for`. See [the project page](https://github.com/sethvargo/bootstrap_forms) for more information |
| [`mail`](https://github.com/mikel/mail) | Used to verify email address supplied by our users are of a valid format. Again [build on a previous example](/2013/03/14/validators-in-rails-using-email-as-an-example.html) |
| [`draper`](https://github.com/drapergem/draper) | Used to encapsulate the presentation of model fields. The [project page](https://github.com/drapergem/draper) contains good documentation |
| `ActiveRecord::Observer`|To detect the edge changes in `User` state and send a confirmation request email when appropriate. The docs [provide a good starting point](http://api.rubyonrails.org/classes/ActiveRecord/Observer.html) |
| `ActionMailer`|To send the confirmation request email. The [Ruby on Rails Guide](http://guides.rubyonrails.org/action_mailer_basics.html) is the best place to start |

The commited code is setup to work with [OpenShift](https://openshift.redhat.com/)

A complete walk-through of the example will be posted at a later date, but for now the entire project is [posted to
GitHub](https://github.com/myitcv/email-confirm-state-example)

