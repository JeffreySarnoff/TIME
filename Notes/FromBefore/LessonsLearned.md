### A few lessons learned making accurate, high performance datetime stuff
#####    Jeffrey Sarnoff  May 5, 2012

#### (1) Users care about accuracy, speed and resolution
> For important classes of apps (e.g. financial market analysis), it matters that    
datetime arithmetic is accurate and fast [no, faster].    
>    
> For other apps (e.g. LAN packet analysis, high energy physics), it matters that    
datetime arithmetic is accurate, fast, and high resolution [no, higher].

#### (2) Nobody has requested sub-second resolution with dates before 1900.
> The astrophysics community has found that it takes a pair of 64-bit values    
to work with Julian dates at current levels of accuracy.    
> GPS time signals are accurate to within (less than) 32 nanoseconds.

*If representation compatible, having a long span datetime and higher resolution modern datetime is one good approach.*

#### (3) With time-of-day, timezones matter
> (even when they are elided) and their robust, portable use will rely on iana's time zone database.    
> Daylight savings time means that once a year, an hour is traversed twice --     
that ambiguity should be resolved (by user input, perhaps with a default), not presumed.ge
>    
> UTC is as a 'common denominator' when working with timezones.     
> Multiple conversions are much less error-prone when storing datetimes as, say,    
(UTC, timechange, isDST, zone) and converting to local time when show()ing it.
>    
> Indexing each timezone in the iana database requires 9 bits.    
>  Encoding timechange to/from UT in 15min increments requres another 8 bits   
(9 if timechange is for Standard Time and a separate bit is used to reflect DST --    
   a more flexible representation).
> An implementation, might use 16 msbs for maskable timezone, and   
16 lsbs for timechange in, say, 4sec units + DST bit.

#### (4) Leap seconds exist.
> Most datetime systems ignore them for speed, which is ok    
only if the user does not care about timespans that resolve < 1min.

#### (5) Use integer types/structures with proper algorithms
>  They generate safer, faster, and more robust for datetime representation.

#### (6) datetime users think of timepoints as moments and as boundriless
>   they think of, timespans (durations), and temporal intervals arithmetically.
