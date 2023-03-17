# puzzle-scheduling
Repository for hoefnagel puzzle club

## Repository Structure
```
.
├── Combined - Puzzle Scheduling - Data Cleaning Feature Engineering.ipynb
├── Combined - Puzzle Scheduling- Feature Importance.ipynb
├── README.md
└── per_member_linreg.ipynb
```

## Features
Our features revolved around a couple of core hypotheses:
  1. Rental behavior is predominantly a personal behavior response, so information about the past actions of a user are important
  2. Harder puzzles (in difficulty or piece count) take longer to complete and therefore will be held longer
  
 With these hypotheses in mind we constructed the following features:
  - pieces_d1 - The number of pieces of easy difficulty
  - pieces_d2 - The number of pieces of average difficulty
  - pieces_d3 - The number of pieces of hard difficulty
  - pieces_d4 - The number of pieces of very hard difficulty
  - member_hold_time_count - The number of previous rentals the member has
  - member_hold_time_mean - The average hold time of previous rentals
  - member_hold_time_std - The standard deviation of the member's historic rentals
  - hold_time_median - The median of the member's past rentals
  - hold_time_rolling_median - The median of the member's last 5 rentals (uses row ordering as temporal order)
  - global_hold_time_mean - The average rental period accross the entire training dataset
