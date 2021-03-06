# Php Search Demo  
[![Build Status](https://travis-ci.com/felfel/php-search-demo.svg?branch=master)](https://travis-ci.com/felfel/php-search-demo)   [![Maintainability](https://api.codeclimate.com/v1/badges/25bbae89f19de24f4ff4/maintainability)](https://codeclimate.com/github/felfel/php-search-demo/maintainability)   [![Test Coverage](https://api.codeclimate.com/v1/badges/25bbae89f19de24f4ff4/test_coverage)](https://codeclimate.com/github/felfel/php-search-demo/test_coverage) 

Dockerized php/laravel search example.

---------------------------------

# Getting Started
### Prerequisites
You only need [docker](https://www.docker.com/get-docker
) and a make tool to get started. Make sure your docker daemon is running.



### Installing

```
git clone https://github.com/felfel/php-search-demo.git php-search-demo/
```
```
cd php-search-demo && make init
```
### Usage
headover to [`localhost:8000/api/search`](http://localhost:8000/api/search)
##### GET Parameters

```
name                searches hotels by hotel name
            e.g.:   http://localhost:8000/api/search?name=media
        
destination         searches hotels by their location (city)
            e.g.:   http://localhost:8000/api/search?destination=cairo
            
price               searches hotels by price range
            e.g.:   http://localhost:8000/api/search?price=90:120
            
date                searches hotels by availabilty in a given date range
            e.g.:   http://localhost:8000/api/search?date=12-5-2013:31-10-2015
            
sort-by             sorts the results by either name or price
            e.g.:   http://localhost:8000/api/search?sort-by=name
            e.g.:   http://localhost:8000/api/search?sort-by=price
            
sorting             can be combined with 'sort-by' to specify sorting, default is ascending
            e.g.:   http://localhost:8000/api/search?sort-by=name&sorting=ascending
            e.g.:   http://localhost:8000/api/search?sort-by=price&sorting=descending
```
All of the parameters are optional and can be combined with each other. Any other parameters will be ignored.

## Running the tests

Make sure you are in the root directory and run
```
make test
```


## Architecture

- Types
  - `DateRange` responsiple for holding a range of dates.
  - `PriceRange` responsiple for holding a range of prices.
  
- Search
  - `BaseQuery` implements `SearchQuery` and responsiple for holding the data to be searched.
  - `FilterDecorator` an abstract implementation of `SearchQuery`, follows a decorator pattern and defines the bones for the different filters, `StringFilterDecorator`, `PriceFilterDecorator` and `DateFilterDecorator`.
  
- Hotel
  - `Hotel\QueryParser` responsiple for parsing hotel search parameters.
  - `Hotel\QueryBuilder` responsiple for building a `SearchQuery` from parsed hotel search parameters (generated from `Hotel\QueryParser`).
  - `HotelUtils` is a facade with helper functions to handle hotel search requests.
  
- Cache
  - `CacheManager` responsiple for the storage/retreival of `ItemChunks` in cache, using different requirements for acquiring an item from cache and actually deleting it. The padded age is used to assure requests that acquired the item can be resolved before deleting the item.
  - `ItemChunks` responsiple for the storage/retreival of big arrays in cache as a list of chunks.

### Assumptions

  - Data provided from the "[3rd party api](https://api.myjson.com/bins/tl0bp)" is always correct and uses the same format. No validation was done on this whatsover.
  - Data provided from the "[3rd party api](https://api.myjson.com/bins/tl0bp)" is volatile, given it includes the avialabilty of the hotel. Hence the low age of the cache.
  - Both the data provider and the client are using the same currency. No currency exchanges implemented.
  
  
### Project setup

* [Docker](https://www.docker.com/) - Containerization Technology
* [Php/Laravel](https://laravel.com/) - The web framework used
* [memcached](https://memcached.org/) - In-memory cache layer
* [nginx](https://www.nginx.com/) - Overkill web server

All of the source code can be found under the `app/` directory.


---------------------------------


- Tests are yet to be completed.
