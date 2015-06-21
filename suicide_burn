//suicide_burn
//
//suicide burns to land a ship
//
//calls on gravity_calculator, suicide_burn_calculator
//
//bug where some craft, mainly heavy ones, crash even with the safety
//I think this is due to the constant acceleration approximation used

//runs required functions
run gravity_calculator. //gravity calculation function
run suicide_burn_calculator. //suicide burn calculation function

//set to known state
clearscreen.
SAS off.
print "Initialising" at (5,4).
wait 1.
SAS on.
RCS off.
gear off.
brakes off.

//set parameters for landing
set vLand to -4.
set runmode to 1.
set thrust to ship:maxthrust.
set counter to 0. //for checking if landed

//find ship parameters
lock h to alt:radar.
lock vd to ship:verticalspeed.
lock vDiff to (vLand - vd).

//calculates values for landing
lock TimeToBurn to ((suicide_burn_calculator(1.1)-h)/vd).
lock ZeroAccelThrust to ((ship:mass*g)/thrust).

//loops until landed
until runmode = 0 {
	set g to -gravity_calculator().
	
	//initial setup for high atmosphere
	if runmode = 1 {
		set TVAL to 0.
		if h > 5000 {
			lock steering to retrograde.
		}
		if h < 50000 {
			brakes on.
			lock steering to srfretrograde. //switches to surface velocity
			set runmode to 2.
		}
	}
	
	//suicide burn start
	if runmode = 2 {
		if h < (suicide_burn_calculator(1.1) - vd) {
			set TVAL to 1.
			RCS on.
		}
		else {
			set TVAL to 0.
		}
		if h/-vd < 2 {
			gear on.
			brakes off.
		}
		if vd > -6 {
			lock steering to up.
			set runmode to 3.
		}
	}
	
	//suicide burn final approcach
	if runmode = 3 {
		if vDiff > 0 {
			set TVAL to (ZeroAccelThrust + 0.1).
		}
		if vDiff < 0 {
			set TVAL to (ZeroAccelThrust - 0.1).
		}
		if vd > -0.1 and h < 100 {
			set counter to counter + 1.
			wait 0.1.
		}
		if counter > 20 {
			set runmode to 0. //if landed for 2 seconds then exits
		}
	}
	
	lock throttle to TVAL.

	//parameters to display
	print "Vertical velocity:			" + round(vd, 2) + "			" at (5,4).
	print "Required burn height:			" + round(suicide_burn_calculator(1.1), 2) + "			" at (5,5).
	print "Time to required burn:			" + round(TimeToBurn, 2) + "			" at (5,6).
	print "Run mode:			" + runmode + "			" at (5,7).
	print "Gravitational acceleration:			" + round(g, 2) + "			" at (5,8).
}

//landing notification and program exit
if runmode = 0 {
	set TVAL to 0. //sometimes throttle was left on so I added this
	unlock throttle. //sometimes throttle was left on so I added this
	clearscreen.
	print "Landing Complete" at (5,4).
	wait 1. //wait to make sure throttle is set to 0
}
