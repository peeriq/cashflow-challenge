# cashflow-challenge


PeerIQ is building out our engineering team and we are seeking exceptional candidates to fill the
role of either Full Stack Engineer or Backend Engineer. We have created this test as an alternative
to traditional coding challenges. We believe that artificial problems with no
connection to our subject matter or to the types of technical challenges we face day to day at
PeerIQ aren't useful in evaluating candidates. Instead, we've created this test to let you show off
your skills and problem solving techniques.

## The Problem

Amortization is paying off a loan over a set period of time in equal installments. Part of each
payment goes to the loan principal and part goes to the interest. Typically, when you take out
a loan, the majority of the payment is applied towards the interest. As you make more payments a larger
proportion of each payment is applied towards the principal.

The goal here is to create a mini product which allows a user to input the loan amount, term and
interest rate (APY) and output the amortization schedule. The product will consist of two components:

- A backend web server which will supply a RESTful API and render results in the format specified below
- A frontend application will will make calls to the backend and use the renturned data to render
a webpage

The interest

## Requirements

### Backend

The Backend service should receive, as input, the following fields:

- Loan amount (or principal)
- Interest rate (expressed as an annual percentage rate or APY)
- Term

And return, as output, the following:

- Month
- Starting balance
- Fixed Payment
- Interest Payment
- Principal Payment
- Ending balance
- Total interest


The first step is to compute the fixed payment amount. This can be done using the following formula:

![equation](https://latex.codecogs.com/gif.latex?A%20%3D%20P%20%5Cfrac%7Br%281&plus;r%29%5En%7D%7B%281&plus;r%29%5En%20-%201%7D)

Where `A` is the payment amount per period, `P` is the intial principal (the loan amount, in our example
$250,000), `r` is the interest rate per period (in this case 3.625% / 12 or 0.00302083333 (approx. 0.302%)). `n` is the number of payments, in this case 360 months (30 years * 12 months / year).

Once the fixed payment amount is computed, we can compute the remaining fields.
For example, in a loan where the loan amount is $250,000, interest rate is 3.625% and term is 30 years, the
above fields can be computed as follows:

Field Name | Description
--- | ----
Month | The month of the payment, e.g., first payment, month = 1
Starting balance | The starting balance, e.g., for the first month $250,000, otherwise, this will be the ending balance of the *previous* month. For month = 2, the starting balance will be the ending balance of month = 1
Fixed payment | The fixed payment amount computed above. Note! This will be the same in every payment!
Interest payment | The interest payment for this period, computed as the starting balance times the APY divided by 12
Principal payment | The principal payment for this period computed as the fixed payment minus the interest payment
Ending balance | The starting balance minus the interest payment and the principal payment
Total interest | The total interest paid, computed as a rolling sum of all the interest payments to date

This example computation with the above parameters ($250,000 principal, 30 year term, 3.625% APY)
is included (complete with formulas and explanation) as an example file
in `resources/`.

The Backend service should return a JSON blob consisting of a list with the above fields, one
for every single month. For example, a loan with a ten year term would have 10 years * 12 months / year
payments or 120 entries. Each entry should generally follow the following format:

```
{
    'month': 1,
    'starting-balance': 250000,
    'fixed-payment': 1140.13,
    'principal-payment': 384.92,
    'interest-payment': 755.21,
    'ending-balance': 248859.87,
    'total-interest': 755.21
}
```

## Submission

When you're ready to submit your work, zip the source directory
and email it to your HR contact. We'll take a look and respond shortly!
