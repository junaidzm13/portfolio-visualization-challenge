# Pie chart (portfolio visualization)

### Description

Over the past few months, your friend, Joe, has bought several financial instruments. As his investments grew so 
did the difficulty to track his portfolio's risk. As a result, he decides to build several visualizations to track his
portfolio's proportions and associated risks. Since, he is a really close friend of yours, you agree to help.

You are given a portfolio made up of **N** instruments, each instrument having the following attributes:
  - quantity
  - price
  - currency
  - sector
  - assetClass
  - region

---

### First visualization

For the first visualization, your friend would like you to build a pie chart to help visualize the amount invested in
each instrument.

Your task is to calculate `N + 1` angles (in `radians`) representing the boundaries of the slices of the pie chart, where each
slice represents the relative investment allocation for an instrument. The slices should be arranged in descending order
of their sizes (instrument's proportion) in clockwise direction.
You should return a list containing `N + 1` angles where `1 <= N <= 2000`.

Since, it will be hard to visualize investments with very small proportions, your friend adds the following constraint:
*if any instrument's invested proportion makes up less than **0.05%** (or 0.00314159 radians), its slice/arc must be
assigned the corresponding minimum value in radians and the angles of other slices/arcs must be adjusted proportionally.*

##### Example

Portfolio A is made up of 3 instruments S1, S2 and S3 with the following proportions 40%, 30% and 30% respectively. 
Now if we draw a pie chart sorted by the invested value of securities we get:

![Part1-visualization.png](src/main/resources/static/Part1-visualization.png)

Then the program should return the following list:

```
[
    0, // start of pie chart
    2.5132, // boundary of slices of S1, S2
    4.3982, // boundary of slices of S2, S3
    6.2832 // end of pie chart
]
```

##### Example 1

**Input**
```json
{
  "instruments": [
    {
      "quantity": 4,
      "price": 5.0,
      "currency": "HKD",
      "sector": "ECommerce",
      "assetClass": "Equity",
      "region": "APAC"
    },
    {
      "quantity": 5,
      "price": 4.0,
      "currency": "JPY",
      "sector": "Finance",
      "assetClass": "FixedIncome",
      "region": "APAC"
    },
    {
      "quantity": 10,
      "price": 6.0,
      "currency": "EUR",
      "sector": "Education",
      "assetClass": "Derivatives",
      "region": "EMEA"
    }
  ],
  "part": "FIRST"
}
```

**Output**
```json
{
  "instruments": [
    0,
    3.76991118,
    5.02654825,
    6.28318531   
  ]
}
```

---

### Second visualization

For the second visualization, your friend's proposal is to build a split chord diagram to help visualize not only the
relative amount invested in each instrument but also the risk exposures under each of the following categories:
  1. currency
  2. sector
  3. assetClass 
  4. region

Your task again is to calculate angles (in radians).

#### Split Chord Diagram (only for reference)
![Part2-visualization.png](src/main/resources/static/Part2-visualization.png)

For the above diagram, only consider the outer skeleton (ignoring the connectors/ribbons). On the left-hand side,
arks are ordered by size (ascending, clockwise) and each ark represents an instrument in the portfolio with the size of arc proportional to
the value invested in the instrument.

On the right, we have four main arcs of same size representing the value invested in each category. Each main ark is
then further divided into smaller arcs *(ordered by size, clockwise descending)* representing the proportion invested in each type under a given category
(if category is `currency`, then `type` would be `USD, HKD, JPY, etc.`). 

 - Assume that on the right the arks are in the following order (clockwise): `currency, sector, assetClass, region`.
 - Space between main arks on the right is `0.00314159 radians`
 - Tops of the left and right side should be symmetric and `PI / 3 radians` apart.
 - Bottoms of the left and right side should be symmetric and `PI / 3 radians` apart.

As before you have to calculate `N + 1` angles representing relative investment allocations for each instrument in the
portfolio. In addition to that, given `c, s, a, r` different types of currencies, sectors, asset classes and regions,
you also need to calculate `c + 1, s + 1, a + 1, r + 1` angles making up the four main arcs representing each risk category.
Where `1 <= N <= 500, 1 <= c <= 8, 1 <= s <= 9, 1 <= a <= 6, 1 <= r <= 5`


Again, since it will be hard to visualize investments with very small proportions, please consider the following constraint
in your calculations: if an arc has an angle smaller than **0.00314159**, it must be re-assigned the corresponding 
minimum value in radians and the angles of other arcs must be adjusted proportionally.

#### Example 1

**Input**
```json
{
  "instruments": [
    {
      "quantity": 4,
      "price": 5.0,
      "currency": "HKD",
      "sector": "ECommerce",
      "assetClass": "Equity",
      "region": "APAC"
    },
    {
      "quantity": 5,
      "price": 4.0,
      "currency": "JPY",
      "sector": "Finance",
      "assetClass": "FixedIncome",
      "region": "APAC"
    },
    {
      "quantity": 10,
      "price": 6.0,
      "currency": "EUR",
      "sector": "Education",
      "assetClass": "Derivatives",
      "region": "EMEA"
    }
  ],
  "part": "SECOND"
}
```

**Output**
```json
{
  "instruments": [
    3.66519143,
    4.08407045,
    4.50294947,
    5.75958653
  ],
  "currency": [
    0.52359878,
    0.83634432,
    0.94059284,
    1.04484136
  ],
  "sector": [
    1.04798295,
    1.36072850,
    1.46497701,
    1.56922553
  ],
  "assetClass": [
    1.57236712,
    1.88511267,
    1.98936119,
    2.09360970
  ],
  "region": [
    2.09675130,
    2.40949684,
    2.61799388
  ]
}
```
------

# Everything below is to be deleted

As such, the `ChallengeRequest` implementation should be something ready for Jackson to convert,
bearing in mind to use `@get:JsonIgnore` on any attributes you do not want to expose.

## Local testing

For testing, write your own solver controller
(remember to run `App` with the environment variable `ENDPOINT_SUFFIX` defined).

Make sure your solver controller handles `POST` requests at the same path as `ENDPOINT_SUFFIX`.

Then, in your browser, go to `http://localhost:8080` (default port), open the console and run the following `fetch`
command:

````javascript
fetch('http://localhost:8080/evaluate', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ "runId": "test", "teamUrl": "http://localhost:8080/", "callbackUrl": "http://localhost:8080/coordinator" }),
})
.then(response => response.text())
.then(data => { console.log('Success!', data); })
.catch((error) => { console.error('Error:', error); });
````

## Example controller

It is strongly recommended to have a controller that serves an example of the challenge request/response.

By default, the endpoint `/example` is exempted from authentication requirements in production, so feel free to use
that.

The `GET` response for this can be something like:

````json
{
  "request": {
    "challenge": "request"
  },
  "response": {
    "challenge": [
      "response",
      "as",
      "desired"
    ]
  }
}
````

## Gitlab CI/Heroku

This template has Gitlab CI set up for easy deployments to Heroku via [`dpl`](https://github.com/travis-ci/dpl). By
default, only `master` branch will be deployed.

You will need to add the following CI/CD variables under Settings:

- `HEROKU_APP_NAME`
- `HEROKU_API_KEY` (Remember to set this variable as masked so that it will not be printed in build logs)

To perform deployments for branches, do not set the variables as protected so that the pipeline is able to access the
variables.

If you wish to have multiple environments, you can add in CI/CD variables with environment scope, and add a new job
in `.gitlab-ci.yml` extending `.deploy_template` for the new environment(s). A sample set-up can be found in
branch `gitlab-ci-demo`.
