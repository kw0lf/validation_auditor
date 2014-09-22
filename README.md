# Validation Auditor

[![Build Status](https://travis-ci.org/watu/validation_auditor.png?branch=master)](https://travis-ci.org/watu/validation_auditor)
[![Coverage Status](https://coveralls.io/repos/watu/validation_auditor/badge.png?branch=master)](https://coveralls.io/r/watu/validation_auditor?branch=master)
[![Code Climate](https://codeclimate.com/github/watu/validation_auditor.png)](https://codeclimate.com/github/watu/validation_auditor)
[![Inline docs](http://inch-ci.org/github/watu/validation_auditor.png?branch=master)](http://inch-ci.org/github/watu/validation_auditor)
[![Gem Version](https://badge.fury.io/rb/validation_auditor.png)](http://badge.fury.io/rb/validation_auditor)
[![Dependency Status](https://gemnasium.com/watu/validation_auditor.svg)](https://gemnasium.com/watu/validation_auditor)

A user visits your web app, tries to do something with it but it fails due to a validation error. Generally, the
validation is stopping a user from doing something bad, but every now and then it's the validation that is bad. Don't
you hate it when the credit card processor won't accept your name as written in the credit card due to an unexpected
character and won't accept anything else because that's not your name? We all do.

This gem allows you to easily keep a log of validation errors, so you can later inspect them to try to find those cases
where things are going wrong.

This gem supports 
[Rails 3.2, 4.0 and 4.1 running on Ruby 1.9, 2.0 or 2.1](https://travis-ci.org/watu/validation_auditor) (latest stable
version of each).

## Installation

Add this line to your application's Gemfile:

    gem "validation_auditor"

And then execute:

    $ bundle

You need to install the migration file in your Rails project, which you can do by:

    $ rails generate validation_auditor:install

## Usage

After you run the migration, you need to enable the validation auditor in each model by calling the class method:
`audit_validation_errors`, for example:

    class Blog < ActiveRecord::Base
      audit_validation_errors
    end

From then on, every time saving that record fails, a record will be saved to validation_audits with the failure message
and some extra information.

If you enable validation audit on the controller, by calling `audit_validation_errors` as in:

    class BlogsController < ApplicationController
      audit_validation_errors
    end

then you'll also get params, url and user agent in the validation audit. This breaks the model-controller separation, so
it's optional.

If for some reason saving a validation audit fails, the exception will be left to propagate into your application so
that no exception is silently swallowed. You may want to not let a secondary system, like auditing, to stop your
application for working (depending on how critical auditing is for you). If that's the case, you can define an
exception handler that can report in whatever fashion you normally report exceptions to your dev team or silently
swallow the exception. This may or may not work in Rails < 4.

    ValidationAuditor.exception_handler = lambda do |e, va|
        puts "When trying to save validation audit #{va}, exception #{e} was encountered."
    end

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
