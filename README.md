#BDAstroCalc

###Written By: Ian McKay at Braindrizzle Studio:tm:

###Based on the work of:

(JavaScript) https://github.com/mourner/suncalc/blob/master/suncalc.js
(IDL) http://idlastro.gsfc.nasa.gov/ftp/pro/astro/mphase.pro
(Math) http://aa.quae.nl/en/index.html


##What is it?

BDAstroCalc is [currently] a simple struct, written in Swift, for low-precision calculations of commonly needed properties of Earth's sun and moon, such as rise/set times, altitude/azimuth, declination/right-ascension, etc.. It was created because while there are such things in other languages, I couldn't find one written in Swift. The project for which I wrote it didn't require any precision so this was sufficient; if there's any demand then I'll be happy to update BDAstroCalc to be more sophisticated.

The iOS project/demo is not meant to be a completed app. It is simply to demonstrate the results and to provide an example of use.


##Documentation

I have tried to provide fairly thorough commenting throughout the file. Do pay attention to the notes below; the work on which this was based sometimes does not abide by standards, so a couple of conversions need to be done if you're presenting the results. (The Demo labels reflect this.) I will update the methods to follow standards if there are any requests for it.


##Notes on Comments

The SunCalc code, which server as the basis for BDAstroCalc, was JavaScript; its reference code (when you dig back) was largely in IDL written for the USNO and NASA. The USNO/NASA commenting was fairly thorough, but over the translations the commenting has gotten more and more sparse. I've tried to decipher what's what, but I may have guessed wrong here or there--I'll happily update anything you notice that needs updating. As with any astronomical calculations there are a LOT of magic numbers; I've tried to include references to explanations where I've found them (though this process will be ongoing).


##Other Notes

1) Pay close attention to your timezone after retrieving results. If you look through the SunCalc Github issues, many were just timezone-related.
  
2)
  a) In SunCalc everything returns in radians EXCEPT the longitude and latitude of moonPositon, moonRiseAndSet, sunPosition, and sunRiseAndSet. 
    To reduce confusion I've made those that take degrees require CLLocationCoordinate2D; 'longitude' and 'latitude' parameters are always in radians. 
    All returns are in radians.

2)
  b) A further complication: it seems that for convenience reasons longitude was made to be the "longitude west of the prime meridian" and latitude the "latitude north of the equator".
    The latter is normal, of course, but the former is negating the norm. This applies only to non-CLLocationCoordinate2D longitude. This won't matter if you're only using moonPositon, moonRiseAndSet, sunPosition, and sunRiseAndSet. 

2)
  c) If for some reason you don't want to import CoreLocation you can simply change the relevant function definitions to use longitude/latitude (in degrees).

3) Azimuth here is measured from South to West; the norm is from North to East. Just add/subtract Pi to the result to match with results elsewhere.

4) The moon rise and set vary a lot over time so that sometimes the rise is on the day before the set, sometimes the rise is on the day after the set, sometimes there is a set and no rise, sometimes a rise and no set. The returned results will reflect that: the day of the rise and set might be different.


##Installation

Simply copy BDAstroCalc.swift into your project.


##Use

Everything is static so that you don't need to instantiate--just call the methods on the struct itself.


##Examples

The demo app code will serve as the most useful example. To use BDAstroCalc simply send the messages to the struct itself:

```Swift
let myDate = NSDate()
let myLocationCoordinates = CLLocationCoordinates2D(latitude: myLatitude, longitude: myLongitude)
let moonRiseAndSetTimes = BDAstroCalc.moonRiseAndSet(date: myDate, location: myLocation)
println(moonRiseAndSetTimes.rise) // Prints the NSDate of the moon rise 
```

##Issues

Please feel free to report issues, or offer suggestions, here on GitHub. Alternatively: email  support@braindrizzlestudio.com