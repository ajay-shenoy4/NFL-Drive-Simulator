This code builds a full NFL drive simulation engine using ten seasons of nflfastR play-by-play data (2015–2025). 
It begins by loading, cleaning, and filtering offensive plays, manually computing yards gained and preparing modeling features. 
A logistic regression model predicts run vs. pass tendencies, while an XGBoost regression model predicts yards gained for any play state. 
Additional models estimate field-goal success, fourth-down conversion probability, and typical punt outcomes using historical data.
With these components, the script constructs an Expected Points (EP) framework for go-for-it decisions and implements play-level simulations that include noise, sacks, turnovers, and clock management.
Building on this, it simulates entire drives, dynamically choosing between passing, running, punting, field-goal attempts, and fourth-down aggressiveness, producing realistic drive results such as touchdowns, field goals, turnovers, punts, or end-of-half scenarios.
