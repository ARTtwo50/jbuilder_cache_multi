# JbuilderCacheMulti

Adds cache_collection! method, useful when iterating over a collection. The main advantage is that it will try to use fetch_multi from rails (if available in rails and supported by the cache) to query the cache. 

fetch_muti uses read_multi (supported by memcache) to retreive multiple items in one go. This means less queries to the cache == faster responses. If items are not found, they are writen to the cache (individualy, at the moment).

## Installation

Add this line to your application's Gemfile:

    gem 'jbuilder_cache_multi'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install jbuilder_cache_multi

## Usage

Caches a collection of objects using fetch_multi, if supported (otherwise iterates over the collection using fetch)
Requires a block for each item in the array. Accepts optional 'key' attribute in options (e.g. key: 'v1').

Note: At the moment, does not accept the partial name as an argument #todo

Examples:

	json.cache_collection! @people, expires_in: 10.minutes do |person|
	  json.partial! 'person', :person => person
	end

Or with optional key

	json.cache_collection! @people, expires_in: 10.minutes, key: 'v1' do |person|
	  json.partial! 'person', :person => person
	end
  
  
## Todo

- Add support for passing a partial name as an argument (e.g. json.cache_collection! @people, partial: 'person') or maybe even just "json.cache_collection! @people" and infer the partial name from the collection...

- When rendering other partials, use the hash of THAT partial for the cache_key (I beleieve it currently uses the view from where cache_collection! is called to calculate the cache_key) #not_good

## Contributing

1. Fork it ( https://github.com/joshblour/jbuilder_cache_multi/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request

## Credit
Loads borrowed from https://github.com/n8/multi_fetch_fragments. Thank you!
And of course https://github.com/rails/jbuilder