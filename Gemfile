# frozen_string_literal: true
source 'https://rubygems.org'
ruby ENV['CUSTOM_RUBY_VERSION'] || '2.2.3'

# Bundle edge Rails instead: gem 'rails', github: 'rails/rails'
gem 'rails', '4.2.7'
gem 'puma', '2.16.0'
gem 'awesome_print'
gem 'dotenv-rails'
gem 'multi_json'
gem 'httparty'
gem 'sentry-raven'
gem 'oauth'
gem 'hipchat', github: 'hipchat/hipchat-rb'

group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug'

  # Access an IRB console on exception pages or by using <%= console %> in views
  gem 'web-console', '~> 2.0'

  # Spring speeds up development by keeping your application running in the background. Read more: https://github.com/rails/spring
  gem 'spring'
  gem 'spring-commands-rspec'

  gem 'rspec-rails'
  gem 'guard'
  gem 'guard-rspec'

  gem 'rf-stylez'
end

group :test do
  gem 'simplecov'
  gem 'vcr'
  gem 'webmock'
end
