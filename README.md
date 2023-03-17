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

## Model Performance

Our model performed reasonably well on Users that had substantial rental history. We utilized a linear model over our features and found that the highest coefficients were on the mean hold time for a user and the 5-period rolling median hold time. We do still see meaningful coefficients on the various puzzle piece-difficulty features indicating that they may help predict user rental time, but that a larger impact comes from behavioral features such as average and rolling median hold times.

<img width="672" alt="Screenshot 2023-03-16 at 10 50 58 PM" src="https://user-images.githubusercontent.com/26069835/225823532-67587ed8-f630-470e-934c-6087ec2b50c7.png">

```
Index(['pieces_d1', 'pieces_d2', 'pieces_d3', 'pieces_d4',
       'member_hold_time_count', 'member_hold_time_mean',
       'member_hold_time_std', 'hold_time_median', 'hold_time_rolling_median',
       'global_hold_time_mean'],
      dtype='object')
Universal Coef [ 1.82406926e+00  2.38836643e+00  2.38540074e+00  1.49342888e+00
 -7.07107059e-02 -2.85327726e+00  7.98893400e-03 -4.08959826e+00
  8.37142834e+00  0.00000000e+00]
Newps Coef [ 1.89533032  2.47786508  2.4765758   1.53903089 -0.12909683 -2.96589424
  0.17861954 -4.05496226  8.3059369   0.        ]
  
 ```
 
 ## Conclusions
 
