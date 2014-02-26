---
date: 2013-06-20
layout: post
title: Using Google APIs in Rails apps with OAuth 2.0
location: London
author: paul
tags:
- coding
- ruby
- rails
- oauth2
- google
---

[Back in February](/2013/02/19/omniauth-google-oauth2-example.html) I wrote a blog post about omniauth + Google OAuth2
on Rails. The app desribed in the post stopped short of doing anything useful once the OAuth2 credentials had been
obtained. The article you are reading revises my current thinking on how best to use OAuth2 + Google in a Rails app, but
importantly goes the extra mile of offering some code that pulls data from the [Google Calendar
API](https://developers.google.com/google-apps/calendar/)

Firstly credits. I bumped into [this article](http://blog.baugues.com/google-calendar-api-oauth2-and-ruby-on-rails) by
Greg Baugues which inspired me to step outside the world of [`omniauth`](https://github.com/intridea/omniauth) and
explore [`google-api-client`](https://github.com/google/google-api-ruby-client) and
[`signet`](https://github.com/google/signet)

`google-api-client` is a gem that "makes it trivial to discover and access supported APIs" - e.g. the Calendar API.
`signet` is an OAuth 1.0 / OAuth 2.0 implementation by the same guys behind the `google-api-client` gem. Combined they
allow you to write a Ruby app that uses OAuth2 to pull data from Google APIs.

The important thing to note here is that `google-api-client` depends on `signet` and uses it extensively for
authentication flows etc. Therefore any Rails app using `omniauth` for authentication would need (at least in part) to
use `signet` before getting down to the Google API bit. So, I hear you ask, why bother with `omniauth` at all? Good
question.

My usage of `omniauth` (not the gem itself) always seemed a bit flawed. 99% of the time I'm looking for an OAuth2
authentication gem that will handle not only the OAuth dance but also the persistence of the retrieved `access_token`
(and optionally `refresh_token`) to some store (generally `ActiveRecord`).

Having concluded this was never going to be the responsibility of an OAuth gem (`signet`, `omniauth` or otherwise) I set
about writing [`signet-rails`](https://github.com/myitcv/signet-rails) , a gem that would make it easy to incorporate an
OAuth2 flow into a Rails app but also handle the persistence aspect I so longed for.

## signet-rails

`signet-rails` is in its early days but will hopefully blossom into a transparent OAuth2 handler within Rails apps,
sitting neatly atop `signet`. [Comments gratefully received](https://github.com/myitcv/signet-rails/issues) over on
GitHub on progress to date.

## Example Rails app

In pulling together what is a _very_ early version of this gem, I have also create a sample app.
[`test-signet-rails`](https://github.com/myitcv/test-signet-rails) demonstrates how to use `signet-rails` and then goes
on to show how to connect to the Google Calendar API and retrieve a list of the user's calendars.

## Refreshing access tokens

Another question that vexed me for some time was how and when to refresh expired access tokens. `omniauth` does not
claim to make any provision for this functionality. Indeed in response to a question on the matter,
[@zquestz](https://github.com/zquestz) (who maintains the `omniauth` Google OAuth2 implementation amongst other things)
[directed me towards `google-api-client`](https://github.com/intridea/omniauth-oauth2/issues/40#issuecomment-19335998)

With `signet-rails`, refreshing of access tokens is indeed left to `google-api-client`. If a user's `access_token` is
refreshed in the course of a request, the updated `access_token` is persisted for use in later requests.

## Conclusion

Whilst very much a work in progress, `signet-rails` provides an easy way to integrate OAuth2 into a Rails app, persist
OAuth2 credentials and then uses those credentials to access Google APIs. It should be noted the gem itself is not
specific to Google (although `v0.0.4` at the time of writing has some hard codings to Google end points) so
theoretically it could be used for any OAuth2 server, and the credentials then used with any OAuth2 driven service.