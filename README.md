# usng-py
Module to calculate National Grid Coordinates (python port)

This is a python port of usng.js (Copyright (c) 2009 Larry Moore, jane.larry@gmail.com). Originally released under the MIT License, this code falls under the same. 

MIT License info:
  http://www.opensource.org/licenses/mit-license.php 
  http://en.wikipedia.org/wiki/MIT_License

Port was conducted by John Zechlin, MotionDSP (http://www.motiondsp.com)

All code should be functionally equivalent to the original .js file (to include any previously undiscovered bugs still in the original). My apologies for any typing errors that may have occurred.  

Original usng.js is included for reference as well as the following interface summary...

*************************************************************************
programmer interface summary

1) convert lat/lng decimal degrees to a USNG string function LLtoUSNG(lat, lon, precision) inputs are in decimal degrees, west longitude negative, south latitude negative
   'precision' specifies the number of digits in output coordinates
         e.g. 5 specifies 1-meter precision (see USNG standard for explanation)
         One digit:    10 km precision      eg. "18S UJ 2 1"
         Two digits:   1 km precision       eg. "18S UJ 23 06"
         Three digits: 100 meters precision eg. "18S UJ 234 064"
         Four digits:  10 meters precision  eg. "18S UJ 2348 0647"
         Five digits:  1 meter precision    eg. "18S UJ 23480 06470"
    return value is a USNG coordinate as a text string
    the return value contains spaces to improve readability, as permitted by 
        the USNG standard
        the form is NNC CC NNNNN NNNNN
        if a different format or precision is desired, the calling application 
            must make the changes

2) convert a USNG string to lat/lng decimal degrees
 function USNGtoLL(usng_string,latlng)
    the following formats of the input string are supported:
        NNCCCNNNNNNNNNN
        NNC CC NNNNNNNNNN
        NNC CC NNNNN NNNNN
        all precisions of the easting and northing coordinate values are also supported
             e.g. NNC CC NNN NNN
    output is a 2-element array latlng declared by the calling routine
        for example, calling routine contains the line var latlng=[]
        latlng[0] contains latitude, latlng[1] contains longitude
           both in decimal degrees, south negative, west negative

3) convert lat/lng decimal degrees to MGRS string (same as USNG string, but with no space delimeters)
  function LLtoMGRS(lat, lon, precision)
   create a string of Military Grid Reference System coordinates
   Same as LLtoUSNG, except that output cannot contain space delimiters;
   NOTE: this is not a full implementation of MGRS.  It won't deal with numbers 
         near the poles, but only in the UTM domain of 84N to 80S

4) wrapper for USNGtoLL to return an instance of GLatLng 
 function GUsngtoLL(usngstr)
   input is a USNG or MGRS string
   return value is an instance of GLatLng 
   use this only with Google Maps applications; USNGtoLL is more generic

5) evaluates a string to see if it is a legal USNG coordinate; if so, returns the string modified to be all upper-case, non-delimited; if not, returns 0 function isUSNG(inputStr)

for most purposes, these five function calls are the only things an application programmer needs to know to use this module.

Note regarding UTM coordinates: UTM calculations are an intermediate step in lat/lng-USNG conversions, and can also be captured by applications, using functions below that are not summarized in the above list. The functions in this module use negative numbers for UTM Y values in the southern hemisphere. The calling application must check for this, and convert to correct southern-hemisphere values by adding 10,000,000 meters.

