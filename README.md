# Route Builder 5000


### Playing With The Geolacation API's

Browers have pretty solid HTML5 support for geolation now. For example, you can open your console right in the browser and type this:

```
var g = navigator.geolocation.getCurrentPosition(g => {
	console.log('got pos ', g);
  return g
},err => {
	console.log('didn't get pos ', err);
  return b
} )
```

(Note: make sure to click "Allow" when you browser asks if you want to share your location).

This returns a "Position" object, containing a corrdinates object and a timestamp.

For example:
```
{
  coords: {
    accuracy: 82,
    altitude: null
    altitudeAccuracy: null
    heading: null
    latitude: 40.745938599999995
    longitude: -73.99596989999999
    speed: null
  },
  timestamp: 1565543870672
}
```

Since nyc is pretty flat, I think we can take altitude out of the equation and think about each location in terms of the latitude and longitude. Then it's not so much different from looking at the points as if they were x and y coordinates on a regular cartesian plane.



## The buildRoute Function

The core function that this service exposes would be called something like, "buildRoute", and as parameters it takes multiple destinations. However, things could be a bit more complicated than that. Here are some assumptions that I am making in the calculations here:

- The "current location" input represents where the person is physically standing at the current time.
- There can be an optional "inital destination" input. If supplied, the person must go here first.
- There can be an optional "final destination" input. If supplied, the person must end the route here.
- There can be an optional 0 or more "intermediate stops". If supplied, the person must visit all intermediate stops exactly once but in any order.


From the information above, we can conceive of a function that looks something like this:

```
static buildRoute( initialPersonLocation: Coordinates, intermediateDestinations?: Array<Coordiantes>, initalDestination?: Coordinates, finalDestion?: Coordinates) : RouteObject   
```

The output RouteObject could have some information about the total route and then info about each destination.

example:
```
{
  routeId: 'ASDFASDF',
  totalLength: '1.1 miles',
  numberOfDestionations: 5,
  destionations: [{
      name: "Target",
      address: "123 Park Ave:,
      distanceFromPrevious: '500ft',
      distanceFromNext: '200ft',
      thingsToGetThere: [{
        ...
      }
    },
    ...
  ]
}
```

