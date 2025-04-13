# Injectors

### The Injectors
The injectors that I selected are FlowForce 640cc injectors.

The data that is available with the injectors is
- Flow rate at 3 bar fuel pressure
- Dead time at 13.2 volts and 11, 12, 13, 14, 15, 16, at both 3 and 4 bar.  

### The tables
There a few tables that are import to the control of the injectors, some are time bases milliseconds (ms) and others are percentages based compensation tables that are designed to adjust the injector pulse width by a percentage under certain conditions.  

*NOTE* the mx5 fuel pressure is 4 bar and most injector data is measured at 3 bar, so you may have to multiply up the stated injector flow rate.


#### Injector Base PW
From what understand this is the PW in milliseconds that is needed to correctly fuel the engine to a 14.7AFR or Lambda 1, at wide open throttle, at 5000 RPM.

I used a handy spreadsheet to get me in the right ball park.  As stated above I had to multiple up the injector flow rate to account for the 4 bar fuel pressure and used 725cc for my calculation.

I used the value calculated in the spreadsheet as my starting point.  Although it needed some refinement (discussed later), it was good enough to get the engine running in the right ball park.

TO DO link the spreadsheet.

#### Injector Cranking PW
This table is again PW in milliseconds and the stock tune has pulse widths that are relevant for the stock injectors.  I simply took the percentage difference in injector size and multiplied all values in the table by the percentage to decrease the PWs.  

Again this worked really well, and I was able to get the car started straight away and did not have to make any more adjustments to this table.

#### Injector Opening Time
a

#### Injector Scaling Global
a

#### Injector Scaling Maybe 2
a

