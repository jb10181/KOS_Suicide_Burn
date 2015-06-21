//suicide_burn_calculator
//
//calculates suicide burn height
//
//calls on gravity_calculator
//
//bugs none

//suicide burn calculation function
function suicide_burn_calculator {
	declare local parameter safety. //1 corresponds to no safety margin, 1.1 has a 10% safety margin
	
	declare local shipmass to ship:mass.
	declare local vd to ship:verticalspeed.
	declare local va to ((thrust/shipmass) + gravity_calculator()).
	declare local A to (vd^2)/(2*va).
	return safety * A. //adds the safety margin for error
}.
