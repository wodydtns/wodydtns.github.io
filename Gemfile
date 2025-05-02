# frozen_string_literal: true

source "https://rubygems.org"

gemspec

gem "html-proofer", "~> 5.0", group: :test
gem 'jekyll-archives'
gem "jekyll-seo-tag", "~> 2.8"
gem "jekyll-sitemap", "~> 1.4"
gem "jekyll-feed", "~> 0.17"
gem "jekyll-paginate", "~> 1.1"
gem "jekyll-redirect-from", "~> 0.16"

group :jekyll_plugins do
  gem 'jekyll-compose'
end


platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.2.0", :platforms => [:mingw, :x64_mingw, :mswin]
