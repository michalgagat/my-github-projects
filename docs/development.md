# Development

Notes for developing this project.

- [Access token](#access-token)
- [Styling](#styling)
- [Reloading](#reloading)
- [Running scripts](#running-scripts)


## Access token

It could be possible to mark the token as optional and return empty data in the custom plugin, but this causes issues in other parts which except a structure.

One way to reduce build time is to comment out the Jekyll Github Metadata Plugin from the gem file or to set the number of repos in the GQL query file to a low number.

Testing the token:

```sh
make check-env

GITHUB_TOKEN=abc make check-env
```

## Styling

The [jekyll-octopod/jekyll-bulma](https://github.com/jekyll-octopod/jekyll-bulma) gem is used. This makes it easy to start using existing templating approach based on [Bulma](https://bulma.io/) component styling. It also means Bulma is instead and locked at a version, which was the main reason for using the theme here.

Bulma links:

- [Form controls](https://bulma.io/documentation/form/general/)
- [Panel](https://bulma.io/documentation/components/panel/) doc - for search bar.


## Reloading

At build time:

- Github metadata - A request is done to Github API through the Jekyll Github Metadata plugin, which adds `site.github` to the templating namespace. This includes details about the owner, the current repo (based on the remote origin if local) and public repos.
- GraphQL request - A request is done to Github GraphQL API using this project's custom plugin.

When saving changes to the repo, the server will restart. The Github metadata will not be fetched again but any changes to changes to templating will be applied. The GraphQL request will also be done - investigation could be done to avoid this happening.


## Running scripts

The ruby script cannot be run directly, only through Jekyll. This seems to be a bug in one of the gems.

More specifically, after happens if `jekyll` is installed (it does install `eventmachine` in vendor but this can't pick it up.)

```
$ bundle exec ruby request.rb
Could not find eventmachine-1.2.7 in any of the sources
Run `bundle install` to install missing gems.
```

This does not help:

```ruby
gem 'eventmachine', '1.2.7'
```
