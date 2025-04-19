# Tuning for forced induction

There is a lot to change in the tune in versatuner, just in preparation for tuning, a lot of it does not impact the fueling or the ignition, but it is vital to get you started.

## Summary of preparation changes
- Scale all tables with load axis - load will exceed 1 once we are into boost, how far will depend on your target boost pressure
- Adjust Closed to Open loop transition - we will need to get into open loop mode much earlier, the point of transition will depend on your setup.
- Increase limits - there are several limits that need increasing.

## IMPORTANT assumption
Before starting, make sure you have a well dialed NA tune.  Ensure any MAP sensor change is dialed in on your NA motor.  Ensure your injectors are changed and dialed in terms of base pulse width and opening times.  Optional - If possible, install changes to the MAF tube and dial in the MAF calibration.

Your car should be running as NA with ideally Long term fuel trims between +/- 5%, zero is best, but it depends on your time. 

The following assumes that you are starting from a well tuned NA map.  Not following the above advice will make tuning for a turbo pretty much impossible, or impossible to get a good tune that works everywhere in the load/rev range.

## NOTE
You will need to turn on Allow Entering Unsafe Values, which is in the Advanced tab in the tune.  Without doing this you won't be able to change many values to be high or low enough.

## Scaling Tables
The tables have a fixed number of cells available on each axis so when rescaling you have to make a decision how many cells you want to use for the extended part of the table.  For example, looking at the Open Loop Part Throttle AFR Target 2 table, we need to move load of 1 to somewhere in the middle and have the maximum load higher.

The good news is versatuner will rescale the values in the table for you, so you just have to provide the new scale.  You do this by clicking the edit axis button under the table and ensure rescale is checked.

### Load Scaling
I recommend start scaling out to 2, you can always adjust this once you understand your car characteristics.  I rescaled to 1, 1.25, 1.5, 1.75, 2. 

NOTE - the table do not all have the same number of cells for load.  Make sure you scale tables of the same size the same way, this will help avoid confusion.

At this point just scale the tables and don't worry about the values yet, we will get to that.

Tables to scale
- Fueling
    - High Load Enleanment
    - Open Loop Part Throttle AFR Target 2 and 3
- Ignition
    - Timing - Closed Loop Knocking
    - Timing - Closed Loop Normal
    - Timing - ECT IAT Adjustment Multiplier
    - Timing - Max 1 and 2
    - Timing - Open Loop Knocking
    - Timing - Open Loop Normal
    - Timing - Warmup Adjustments 1, 2 and 3
- Load Targeting 
    - Volumetric Efficiency Load Multiplier

### Scaling Injector BARO MAP Delta Compensation

This table needs to go from -100 (~14psi of boost) to ~102.  You need to decide on where to put 0 when rescaling.  I went for ...
-100 -75 -50 -25 0 10 20 30	40 55.46 63.33 71.19 79.19 87.06 95.06 102.9

Rescale 0 to 102.9 using the rescale feature, because we want that to be the same as it was for our NA setup.  As a starting point go with 1.25 in -100 and interpolate the values between 1.25 and 1 that should be at 0 manifold relative pressure. 

We will revisit this table later as it becomes important to get our fueling nailed in boost, but there is other work to do first.

## Closed to Open Loop Transition 
We need to the tune to transition to open loop before boost is made, otherwise will be targeting lambda 1 in boost, which is not good.  The specific will depend on your turbo size and set up.  I am running a GT2860 and start making positive boost pressure at about 2250 to 2500 rpm.  

Even though we transition to open loop early we can still target the AFR we want including lambda 1, we just don't want to be in closed loop when we close to boost, because we can't control the AFR and it does not react fast enough.

### Getting transition happening
All the setting are in the Absolute limits section of the tune.  I am not 100% clear on whether the transition happens as soon as one of the limits is reached or if there is some sort of logic to it, but the following worked for me, to get into open loop at about 2250 rpm  Make adjustments as follows
- CL OL APP Threshold - make this 50.
- CL OL APP Threshold 2 and 3 - because I am making boost at 2000-2500 rpm I changed to have everything 2500 and above as 20%, 2000 to be 30%, 1500 to be 50%, 1000 to be 75% and 500 to be 75%
- CL OP Load Threshold 1, 2, 4, 5 - I discovered I was making positive boost at about 0.62 load above 2000 rpm, so I adjusted this able between 1500 and 3500 rpm to be 0.6.
- CL OL Maximum RPM - 2750, the above setting should ensure we transition before 2750, but his is a back up to ensure we go open loop at 2750 at the latest.
- CL OP Minimum RPM - 2250 in my case, this is where you might need to start at 2250 and then understand where positive boost starts, if you have a bigger turbo you may be able to increase this value.
- CL OL Transition Timer - 0.1 at all RPM.  Boost will come on quickly we we don't want to delay here.

You may need to revisit these setting once you do some logging and understand what load is the transition to positive boost, but they should be a good starting point.

## Operating Load Limit 1 and 2
Adjust the values in this table to match what you think your max load will be.  In my case, I see load of about 1.45 at full boost from around 2500 (might need to change this not done WOT runs yet :).  So I updated the table to have a max load of 1.5 from 2500 to 6000.  Interpolate between the value for 1000 and 2500 rpm.

## Ignition Timing
This is a tricky one and needs dealing with carefully.  Ideally we want a safe starting place where we are not going to destroy our engine.  I suggest you do some research on this and figure out what is a good conservative starting point for timing above the load of 1.  ChatGTP is not bad at helping here, but found it was a bit too aggressive and had to take out 2 degs.

Don't forget to update Open and Closed loop tables including the knocking table which should have reduced timing.  

## Load Targeting - Volumetric Efficiency Load Multiplier
Initially I just added add values that were over 1 and increased as the RPM and load increased, my initial max was about 1.4.  This table will be revisited once we get into boost tuning.  It is this table we need to work on to get our target AFRs aligned with what is actually happening in the engine.

## Fueling
Fueling now becomes a matter of setting our AFR targets, however be warned putting the right values in the able does not mean they will be met when driving!!  We need to do much logging and adjustments first.

### Open Loop Part Throttle AFR Targets 2 and 3
In this table we need to consider when we are making positive boost and adjust accordingly.  I my case, I make boost at about load 0.62 and 2000-2500 RPM, so the target AFR needs adjusting accordingly, so for example my map as AFR 0.84 in the cell for 0.62 load and 2500 rpm because I am just about to make boost, so targeting similar to what WOT would be on a NA engine.  

This table is important to get right as the AFRs are what we will be targeting at parts of cruise, because we changed when we transition to closed loop above.  Set up a sensible set of values as a starting point.  Chat GPT can help here, but remember they are just a starting point.

### Open Loop WOT AFR Target 2 and 3
Only the lower portion of this table should be being used.  Again set up the AFRs you want to target.  Remember these AFRs may not be hit until more tuning is done on the road.

## Remember only change one thing at once past this point.

## Tuning Part 1 - MAF Calibration
If you were not able to calibrate your MAF before bolting on your turbo, then this is first thing to do.  This will need doing by driving, logging, adjusting repeat.

**WARNING** Do not be tempted to go into boost, stay well away, it is likely that your AFRs in boost are way off and probably lean.

Basically drive at low RPM and load until the fuel trims settle, grab the logs and adjust the whole MAF curve by a percentage according to what the total trims are.  I found that my fuel trims were having to put in a lot of fuel.  Adjust the whole MAF curve by a sensible percent, flash and go again logging.  This can take quite a while because you need to wait for the long term fuel trim to stablise after each change.  I found I had to add about 10-15% to my MAF calibration for a 1-2% increase in MAF tube size.  

Once you have the MAF calibration dialed and you are seeing trims in the +/-5% range you are good to move on.  

## Tuning Part 2 - Boost
This is the most risky part of tuning and could cause damage to your engine.  This is what I did, but it is your responsibility, do your own research also.

Generally we need to start a low boost, capture logs, adjust and build up the boost gradually.  At this point we will be looking at 2 things, the actually AFRs against our Fuel table and looking out for knock retard.  Knock can be cause by lean fuel and too much ignition timing.

You don't need to be doing WOT runs at this point, I found I could stay below 4000rpm here, consider setting the rev limiter if needed.

### AFRs
It is unlikely you are hitting your target AFRs in boost.  This is where the Injector BARO MAP Delta Compensation and Volumetric Efficiency Load Multiplier tables comes in.  

We can think of BARO Map Delta Compensation as broad AFR changes across the RPM range for a given boost, so for example, once you get to WOT runs, if you AFR is below your target across the rev range, this table is probably a good place to make adjustments.

We can think of Volumetric Efficiency Load Multiplier as tuning individual load sights, for example if you have a dip in AFR on a WOT pull, but it is otherwise good, an adjustment to this table in the right cell would be the approach to correct that.

We can adjust the injector pulse width when in boost by change the values.  In my case I was too lean, so I increased the multiplier until I was hitting my target AFRs.  Again, this a matter of logging, adjusting, logging adjusting.  

### Knock
If you see knock, I would recommend taking timing out at the appropriate load/rpm cell until the fueling is sorted, you can always try adding it back in once the fueling is sorted.

## So Far
Hopefully at this point your car is driving pretty well, you can accelerate nicely on part throttle, the AFRs are good and you have no knock.  Now what about WOT.  That is to do next.

## Tuning Part 3 - WOT
Hopefully by this point you have gotten to a point where your AFRs are looking ok and you don't have any knock.  Based on your experience so far, you need to make a decision as to whether it is safe to do WOT runs.  I started, only pulling from 2000 - 4000 rpm, logging, correcting, then 2000-5000 rpm and so on.  

At this point don't worry if your AFR is not matching your target, just make sure it is a safe AFR for the boost you are running.  Once we have WOT ok through the rev range, we can come back and adjust the tune to get the target AFR aligned to what is happening.  Use the information above in the AFR section to guide you.

### My Example
When I got to this point, I had lambda 0.76 across the rev range at WOT and no knock.  My target AFR table was 0.73, which is way rich, but I was shooting for safe AFRs.  I now want to converge my targets with reality.  Given I have a stable AFR across the rev range, I will initially add to the BARO compensation at -50, because I am seeing 150kpa at max boost.  This should increase the fueling.  Next I will reduce the AFR targets a little.  I will iterate on this to try and get my target to match reality.  Once it matches, I will then set my targets for real and check by logging.  

You may have to repeat the above for different boost levels to try and dial it in, which will be tricky.  You might need a boost gauge for this.  Using a long hill my help also gather more data points per load sight.