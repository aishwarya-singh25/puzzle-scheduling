# puzzle-scheduling
Repository for hoefnagel puzzle club

## Background
Wooden Jigsaw Puzzle Club is a company based in Port Townsend, Washington, with a library of over 1000 puzzles rented by a network of puzzle enthusiasts worldwide. The ultimate goal for this project was to help the company predict puzzle hold times for individual members in order to reduce shipping costs and optimize customer satisfaction. Currently, when an active member of the club finishes a puzzle, they start a return process on the site, which generates a shipping label for the user to ship this puzzle to the next user’s location in the overall puzzle network. With the addition of this model generated in this project, we will help improve the company’s accuracy in determining the optimal shipping location for a puzzle based on the estimation of the current member’s puzzle hold time.

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

## Best performing approach
<img width="716" alt="Screenshot 2023-03-16 at 10 58 43 PM" src="https://user-images.githubusercontent.com/26069835/225824625-e539b7a1-0d18-4d7e-84b0-05b91081fde2.png">

Our best performing approach involved splitting the user populations to maximize the ability to utilize past rental patterns. For the small subset of users with substantial history (>=75 past rentals) we found decent performance in predicting their next hold-time. To capitalize on this we fit our linear regression to these individuals alone with the hypothesis that fitting the model to just that user would allow a better representation of how puzzle features such as number of pieces and difficulty impacted that particular users hold time. For users without this quantity of history we tested two approaches. The first was to fit a linear regression model to the entire training set, assuming that new users and established users have similar behavior. The Second approach was to segment users without substantial history from the rest of the population and fit our linear regression model to this subset of users. Neither approach showed a significant difference, possibly signaling that new users do not act significantly different from long-time users.

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
 Our work seems to support the idea that hold time of puzzle packs is strongly influenced by behavioral patterns and trends specific to users. Intuitively this may make sense as everyone's schedule is different and the amount of time people have for hobbies varies widely. With this in mind we think more granular timestamps and demographic information could have high yield in learning user behavior and allowing new users to be clustered with existing users to better estimate their behavior.
 
 Our work also supports what we had seen in our literature review that there is a linear relationship between the number of pieces and difficulty of a puzzle and how long a user holds it. However the relationship was a little weaker than we initially expected when compared to behavioral factors. One hypothesis on this is that the relative difficulty of a puzzle compared to a users skill/past-rentals may have a stronger relationship with hold-time than the puzzle difficulty alone. The intuition being that if users take on a challenge significantly outside their normal comfort zone it may have a larger impact on the hold time.
