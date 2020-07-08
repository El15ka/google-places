# Crawler Google Maps
Get data from Google Places that the official [Google Maps Places API](https://developers.google.com/places/web-service/search) does not provide.

## Why?
The official Google Maps Places API is the best option for most use cases.

But unlike the Google Maps Places API, the crawler can provide:

- Popular place times histogram (no data for that in official API)
- Place reviews (up to 5 reviews from official API)
- Place photos (up to 10 photos from official API)

## Bugs, fixes, updates and changelog
This scraper is under active development. Check [CHANGELOG.md](https://github.com/drobnikj/crawler-google-places/blob/master/CHANGELOG.md) for more detailed information
- 2020-07 polygon search and bug fixes

## Usage

If you want to run the actor on Apify platform, you need to have at least a few proxy IPs to avoid blocking from Google. You can use proxy IPs pool on Apify proxy trial or you can subscribe to one of [Apify subscription plan](https://apify.com/pricing).
It is recommended to run the actor with at least 8GB memory. On Apify platform with 8GB memory you can get:
- 100 google place details for 4 compute units
- 100 google place details with images and reviews for 10 compute units - the usage really depends on how many images and reviews specific places have

## Using polygons
For determining polygons to exact search location are used [nominatim maps](https://nominatim.org/).
Currently search is done for first found polygon for combination of input fields: `country`, `state`, `city`.
After finding polygon in nominatim maps, start urls are prepared screen by screen to cover all area. Please be careful with zoom option in this case, as it changes number of steps and accuracy of search.
Each place is also rechecked, whether it is located inside of desired location, as google often find places in nearby locations.

## INPUT
Follow guide on [actor detail page](https://www.apify.com/drobnikj/crawler-google-places) to see how it works.

Example input:
```json
{
  "searchString": "pubs near prague",
  "lat": "50.0860729",
  "lng": "14.4135326",
  "zoom": 10
}
```
On this input actor searches places on this start url: https://www.google.com/maps/search/pubs+near+prague/@50.0860729,14.4135326,10z

- `startUrls` - list of urls with search results to scrape places from.
- `searchString` - String will be search on Google maps. It is possible fill [Google Place ID](https://developers.google.com/places/place-id) in format `place_id:ChIJp4JiUCNP0xQR1JaSjpW_Hms`. Do not include location in search string if you are using polygon localization (`country`, `state`, `city`).
- `searchStringArray` - Array of strings, that will be searched on Google maps. Use if you need to search more different types of places at once.
- `proxyConfig` - Apify proxy configuration, it is required to provide proxy.
- `country` - Country name for polygon localization
- `state` - State name for polygon localization
- `city` - City name for polygon localization
- `maxReviews` - Maximum number of reviews per place
- `maxImages` - Maximum number of images per place
- `lat` - Use it with combination with longitude and zoom to set up viewport to search on.Do not use `lat` and `lng` in combination with polygon localization (`country`, `state`, `city`).
- `lng` - Use it with combination with latitude and zoom to set up viewport to search on.Do not use `lat` and `lng` in combination with polygon localization (`country`, `state`, `city`).
- `zoom` - Viewport zoom, e.g zoom: 10 -> https://www.google.com/maps/@50.0860729,14.4135326,10z vs zoom: 1 -> https://www.google.com/maps/@50.0860729,14.4135326,1z
- `maxCrawledPlaces` - Limit places you want to get from crawler
- `debug` - Debug messages will be included in log.
- `exportPlaceUrls` - Won't crawl through place pages, return links to places
- `forceEng` - Force localization to be in english, some fields are dependent on english and won't work in different language:

You can exclude some attributes from results using input parameters. It can help to speed up crawling.
- set `maxImages` to `0`
- set `maxReviews` to `0`
- set `includeHistogram` to `false`
- set`includeOpeningHours` to `false`
- set`includePeopleAlsoSearch` to `false`
- set `additionalInfo` - Service Options, Highlights, Offerings,.. to `false`

### Country localization
You can force the scraper to access the places only from specific country location. We recommend this to ensure the correct language in results. This works reliably only for US (most of our proxies are from US). Currently, this option is not available in the Editor input , you have switch to JSON input. After you switch, your configuration will remain the same so just update the `proxyconfig` field with `apifyProxyCountry` property to specify the country, example:

```
"proxyConfig": {
    "useApifyProxy": true,
    "apifyProxyCountry": "US"
  }
```

## OUTPUT
Once the actor finishes, it outputs results to actor default dataset.

Example results item:

```text
{
  "title": "The PUB Praha 2",
  "totalScore": 4,
  "categoryName": "Restaurant",
  "address": "Hálkova 6, 120 00 Nové Město, Czechia",
  "locatedIn": Azalea Square,
  "plusCode": "3CGH+F8 New Town, Prague, Czechia",
  "website": "thepub.cz",
  "phone": "+420222940414",
  "temporarilyClosed": false,
  "permanentlyClosed": false,
  "rank": 1,
  "placeId": "ChIJXRQlXoyUC0cRq5R4OBRKKxU",
  "url": "https://www.google.com/maps/place/The+PUB+Praha+2/@50.0761791,14.4261789,17z/data=!3m1!4b1!4m5!3m4!1s0x470b948c5e25145d:0x152b4a14387894ab!8m2!3d50.0761791!4d14.4283676",
  "location": {
    "lat": 50.0761791,
    "lng": 14.4283676
  },
  "searchString": "pubs near prague 2",
  "popularTimesLiveText": "25% busy at .; Not too busy",
  "popularTimesLivePercent": 25,
  "popularTimesHistogram": {
    "Su": [],
    "Mo": [
      {
        "hour": 6,
        "occupancyPercent": 0
      },
      {
        "hour": 7,
        "occupancyPercent": 0
      },
      {
        "hour": 8,
        "occupancyPercent": 0
      },
      {
        "hour": 9,
        "occupancyPercent": 0
      },
      {
        "hour": 10,
        "occupancyPercent": 0
      },
      {
        "hour": 11,
        "occupancyPercent": 20
      },
      {
        "hour": 12,
        "occupancyPercent": 28
      },
      {
        "hour": 13,
        "occupancyPercent": 29
      },
      {
        "hour": 14,
        "occupancyPercent": 24
      },
      {
        "hour": 15,
        "occupancyPercent": 17
      },
      {
        "hour": 16,
        "occupancyPercent": 15
      },
      {
        "hour": 17,
        "occupancyPercent": 18
      },
      {
        "hour": 18,
        "occupancyPercent": 24
      },
      {
        "hour": 19,
        "occupancyPercent": 30
      },
      {
        "hour": 20,
        "occupancyPercent": 37
      },
      {
        "hour": 21,
        "occupancyPercent": 42
      },
      {
        "hour": 22,
        "occupancyPercent": 37
      },
      {
        "hour": 23,
        "occupancyPercent": 25
      },
      {
        "hour": 24,
        "occupancyPercent": 10
      },
      {
        "hour": 1,
        "occupancyPercent": 3
      }
    ],
    ...
  },
  "openingHours": [
    {
      "day": "Monday",
      "hours": "11AM–2AM"
    },
    {
      "day": "Tuesday",
      "hours": "11AM–2AM"
    },
    {
      "day": "Wednesday",
      "hours": "11AM–2AM"
    },
    {
      "day": "Thursday",
      "hours": "11AM–2AM"
    },
    {
      "day": "Friday",
      "hours": "11AM–5AM"
    },
    {
      "day": "Saturday",
      "hours": "6PM–5AM"
    },
    {
      "day": "Sunday",
      "hours": "Closed"
    }
  ],
  "peopleAlsoSearch": [],
  "reviewsCount": 698,
  "reviews": [
    {
      "name": "Robert Nalepa",
      "text": null,
      "publishAt": "a day ago",
      "likesCount": null,
      "stars": 4
    },
    {
      "name": "Polina Cherniavsky",
      "text": null,
      "publishAt": "a day ago",
      "likesCount": null,
      "stars": 5
    },
    {
      "name": "Martin Mudra",
      "text": null,
      "publishAt": "6 days ago",
      "likesCount": null,
      "stars": 4
    }
  ],
  "imageUrls": [
    "https://lh5.googleusercontent.com/p/AF1QipMQKrnbWNFed4bhBaMn_E1hf83ro3af1JT6BuPe=s508-k-no",
    "https://lh5.googleusercontent.com/p/AF1QipNVV1EkzaddM7UsE9bh0KgT5BFIRfvAwsRPVo0a=s516-k-no",
    "https://lh5.googleusercontent.com/p/AF1QipPDAjMIuulyFvHqTWCz_xeQhiDgretyMsHO6Rq_=s677-k-no",
    "https://lh5.googleusercontent.com/p/AF1QipOEsLwms2XreZ7_kzgH_As5SeTfS7jz32ctw5iY=s516-k-no",
    "https://lh5.googleusercontent.com/p/AF1QipPxIaP1YBTpiBNRETq52IG_BC01bhOiJK-xCVVj=s516-k-no",
    "https://lh5.googleusercontent.com/p/AF1QipO4WRAg3nwgyret7jCb1rkgwkmy9sP02m5xNzUk=s387-k-no",
    "https://lh5.googleusercontent.com/p/AF1QipPP2dFuuOfCA4iSmNR_1bhmmkvqcKnezqLGTOjJ=s508-k-no",
    "https://lh5.googleusercontent.com/p/AF1QipOzqk-BheLxt1ojbDvlmNzpMrixcmudyZ17KXhl=s516-k-no",
    "https://lh5.googleusercontent.com/p/AF1QipN1wQ2885ST1qzggCC5AqtmaIMTraYaxZ2uzxV9=s406-k-no",
    "https://lh5.googleusercontent.com/p/AF1QipM5AVgD65Pt1bc0KdpTGan00LQ22kvlOteYOnz2=s677-k-no"
  ],
  "additionalInfo": {
    "Service options": [
      {
        "Takeaway": true
      },
      {
        "Delivery": false
      }
    ],
    "Highlights": [
      {
        "Bar games": true
      },
      {
        "Karaoke": true
      },
      {
        "Live music": true
      },
      {
        "Outdoor seating": true
      }
    ],
    "Offerings": [
      {
        "Beer": true
      },
      {
        "Food": true
      },
      {
        "Vegetarian options": true
      },
      {
        "Wine": true
      }
    ],
    "Dining options": [
      {
        "Breakfast": true
      },
      {
        "Lunch": true
      },
      {
        "Dinner": true
      },
      {
        "Dessert": true
      },
      {
        "Seating": true
      }
    ],
    "Amenities": [
      {
        "Toilets": true
      }
    ],
    "Atmosphere": [
      {
        "Casual": true
      },
      {
        "Cosy": true
      }
    ],
    "Crowd": [
      {
        "Groups": true
      }
    ],
    "Planning": [
      {
        "LGBTQ-friendly": true
      }
    ]
  },
    ...
  ]
}
```
