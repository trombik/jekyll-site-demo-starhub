# frozen_string_literal: true

require 'rbconfig'
source "https://rubygems.org"

git_source(:github) {|repo_name| "https://github.com/#{repo_name}" }

group :jekyll_plugins do
  gem "jekyll-browsersync"
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
install_if -> { RUBY_PLATFORM =~ %r!mingw|mswin|java! } do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

gem "rouge"
gem "html-proofer"
gem "jekyll", "~> 4.0.1"

# The default theme
gem "minima", "~> 2.5"
