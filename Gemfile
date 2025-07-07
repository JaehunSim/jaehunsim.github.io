# frozen_string_literal: true

source "https://rubygems.org"
gemspec

gem "jekyll", ENV["JEKYLL_VERSION"] if ENV["JEKYLL_VERSION"]
gem "kramdown-parser-gfm" if ENV["JEKYLL_VERSION"] == "~> 3.9"
gem "base64"
gem "csv"
gem 'wdm', '>= 0.1.0' if Gem.win_platform?
gem "jekyll-paginate"
gem "jekyll-sitemap"
