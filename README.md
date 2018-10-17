### Geocoder
---

https://github.com/alexreisner/geocoder

```
rails g geocoder:config

rake geocode:all CLASS=YourModel
rake geocode:all CLASS-YourModel REVERSE=true
rake geocode:all CLASS=YourModel SLEEP=0.25 BATCH=100
rake geocode:all CLASS=YourModel LIMIT=1000

geocode 29.951, -90.081

```

```ruby
results = Geocoder.search("Paris")
results.first.coordinates

results = Geocoder.search([48.856614, 2.3522219])
results.first.address

results = Geocoder.search("172.56.21.89")
results.first.coordinates
results.first.country

def address
  [street, city, state, country].compact.join(', ')
end

geocoded_by :address
after_validation :geocode

reverse_geocoded_by :latitude, :longitude
after_validation :reverse_goecode

obj.distance_to(43.9,-98.6)
obj.bearing_to([43.9,-98.6])
obj.bearing_from(obj2)

include Geocoder::Model::Mongoid
include Geocoder::Mondel::MongoMapper

obj.to_coordinates 
obj.coordinates

extend Geocoder::Model::ActiveRecord

Venue.near('Omaha, NE, US')
Venue.near([40.71, -100.23], 50)
Venue.near([40.71, -100.23], 50, units: :km)
Venue.geocoded
Venue.not_geocoded

if obj.geocoded?
  obj.nearbys(30)
  obj.distance_from([40.714,-100.234])
  obj.bearing_to("Paris, France")
end

result = request.location

# config/initializers/geocoder.rb
Geocoder.configure(
  lookup: :yandex,
  ip_lookup: :maxmind,
  api_key: "...",
  timeout: 5,
  units: :km,
  cache: Redis.new,
  cahce_prefix: "..."
)

Geocoder.search("Paris", bounds: [[32.1,-95.9], [33.9,-94.3]])

Geocoder.configure(
  timeout: 2,
  cache: Redis.new,
  yandex: {
    api_key: "...",
    timeout: 5
  },
  baidu: {
    api_key: "..."
  },
  maxmind: {
    api_key "...",
  }
)

add_index :table, [:latitude, :longitude]

include Geocoder::Model::Mongoid
geocoded_by :address, skip_index: true

after_validation :geocode, if: ->(obj){ obj.address.present? and obj.address_changed? }

Geocoder.configure(cache: Redis.new)

Geocoder.configure(cache_prefix: "...")

Geocoder::Lookup.get(Geocoder.config[:lookup]).cache.expire(:all)
Geocoder::Lookup.get(:nominatim).cache.expire("http://...")
Geocoder::Lookup.get(:nominatim).cache.expire(:all)
Geocoder::Lookup.all_services.each{|service| Geocoder::Lookup.get(service).cache.expire(:all)}

geocoded_by :address, latitude: :lat, longitude: :lon
geocoded_by :address, coordinates: :coords

reverse_geocoded_by :latitude, :logitude, address: :loc
reverse_geocoded_by :coordinates, address: :street_address

geocoded_by :address, params: {region: "..."}

geocoded_by :address, lookup: lambda{ |obj| obj.geocoder_lookup }
def geocoder_lookup
  if country_code == "RU"
    :yandex
  elsif country_code == "CN"
    :baidu
  else
    :nominatim
  end
end

reverse_geocoded_by :latitude, :longitude do |obj,results|
  if geo = results.first
    obj.city = geo.city
    obj.zipcode = geo.postal_code
    obj.country = geo.country_code
  end
end
after_validation :reverse_geocode

class Venue
  geocoded_by :address_from_componets
  reverse_geocoded_by :latitude, :longitude, address: :full_address
end

class Venue
  geocoded_by :address,
    latitude: :fetched_latitude,
    longitude: :fetched_longitude
  reverse_geocoded_by :latitude, :longitude
end

Vanue.near("Austin, TX", 200, min_radius: 40)
sw_corner = [40.71, 100.23]
ne_corner = [36.12, 88.65]
Venue.within_bounding_box(sw_corner, ne_corner)

Venue.near([40.71, 99.23], :effective_radius)
Vanue.near("Paris", 50, latitude: :secondary_latitude, longitude: :secondary_logitude)

Geocoder::Calculations.compass_point(355)
Geocoder::Calculations.compass_point(45)
Geocoder::Calculations.compass_point(208)

Geocoder::Calculations.distance_between([47.858205,2.294359], [40.748433, -73.985655])
Geocoder::Calculations.geographic_center([city1, city2, [40.22,-73.99]]. city4)

Geocoder.configure(lookup: :test, ip_lookup: :test)

Geocoder::Lookup::Test.add_stub(
  "New York, NY", [
    {
      'coordinates' => [40.7143528, -74.0059731],
      'address' => 'New York, NY, USA',
      'state' => 'New York'
      'state_code' => 'NY',
      'country' => 'United States',
      'country_code' => 'US'
    }
  ]
)

Geocoder::Lookup::Test.set_default_stub(
  [
    {
      'coordinates' => [40.7143528, -74.0059731],
      'address' => 'New York, NY, USA',
      'state' => ''New York,
      'state_code' => 'NY',
      'country' => 'United States',
      'country_code' => 'US'
    }
  ]
)

Geocoder.configure(always_raise: [SocketError, Timeout::Error])
Geocoder.configure(always_raise: :all)

uninitialized constant Geocoder::Model::Mongoid
uninitialized constant Geocoder::Model::Mongoid::Mongo

Geocoder::Lookup.get(:nominatim).query_rul(Geocoder::Query.new("..."))

Geocoder::Lookup.get(:nominatim).send(:fetch_raw_data, Geocoder::Query.new("..."))

City.near("Omaha, NE", 20, select: "cities.*, venues.*").joins(:venues)
Hotel.near("London, UK", 50).joins(:administrator).preload(:administrator)
```

```

```
