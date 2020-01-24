# Background

PeerIQ is growing and we are seeking exceptional candidates to build out our engineering team!
We have created this test as an alternative
to traditional coding challenges. We believe that artificial problems with no
connection to our subject matter given under artificial conditions (no internet access, unrealistic time constraints) 
aren't useful in evaluating candidates. Instead, we've created this test to let you show off
your problem solving skills!

A few things to consider before diving in:

1. **Completing the challenge will take time.** The test is designed to give us a lot of information we
need to know about you! Your onsite interview will focus largely
on the process and design decisions that you made while completing the challenge. 
2. **There is no requirement on any specific technology used in this challenge.** Use your favorite
language or favorite web framework. There's no requirement that you use any specific technology to
accomplish the challenge!
3. **Show your work!** Comment your code liberally to indicate your thought process. All designs
have certain technical advantages and trade offs. Make sure you add these as comments in the code
to let us know what you're thinking! Include readmes and architecture drawings where possible!
4. **Questions?** This is a complicated challenge and there will inevitably be parts
that are unclear or even mistakes in the documentation or resources. If you have a comment, question
or concern about the challenge and want to let us know, 
feel free to [open an issue](https://github.com/peeriq/cashflow-challenge/issues/new) in this repository!
We promise to respond!

# Cashflow Challenge

Amortization is paying off a loan over a set period of time in equal installments. Part of each
payment goes to the loan principal and part goes to the interest. Typically, when you take out
a loan, the majority of your initial payments are applied towards the interest. As you make more payments a larger
proportion of each payment is applied towards the principal.


# Requirements

The goal is to create a mini product which allows a user to input the loan amount, term and
interest rate (APY) and output the amortization schedule. The product will consist of two components:

- A backend web server which will supply a RESTful API and render results in the format specified below
- A frontend application will will make calls to the backend and use the returned data to render
a webpage that visualizes the amortization schedule




## Backend

The Backend service should receive, as input, the following fields:

- Loan amount (or principal)
- Interest rate (expressed as an annual percentage rate or APY)
- Term (in years)

And return, as output, the following:

- Month
- Starting balance
- Fixed Payment
- Interest Payment
- Principal Payment
- Ending balance
- Total interest


**Example.** Suppose we wanted to compute the amortization schedule for a loan with the following characteristics:
- Loan amount = $250,000
- Term = 30 years (360 months)
- Interest rate = 3.625%


The first step is to compute the fixed payment amount. This can be done using the following formula:

![equation](https://latex.codecogs.com/gif.latex?A%20%3D%20P%20%5Cfrac%7Br%281&plus;r%29%5En%7D%7B%281&plus;r%29%5En%20-%201%7D)

Where `A` is the payment amount per period, `P` is the intial principal (the loan amount, in our example
$250,000), `r` is the interest rate per period (in this case, 3.625% / 12 or 0.00302083333 (approx. 0.302%)).
`n` is the number of payments, in this case 360 months (30 years * 12 months / year).

Once the fixed payment amount is computed, we can compute the remaining fields.
In our example, the above fields can be computed as follows:

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

If you're applying for the Backend engineering position, you're done! Jump forward to Submission to see how to
submit your response.

## Frontend

The Frontend will consist of two components:

1. An input page that will allow the user to input the Loan Amount, Interest rate and Term
2. An output page that will display the amortization results. This output can take any form. You may decide to go
with a tabular layout, simply listing each month's data as a row in a larger data frame. Or you may decide to go with
a more visual representation like a plot or graph. Presentation is up to you!

There are only a couple front end requirements:

* Your Frontend code **must** call the Backend that you've built in order to run the calculation, e.g., the input
page will make a call to the Backend which will receive the call, process the request and output the result. The 
Frontend will receive this result and render the page appropriately.
(For this reason, it's suggested that you tackle building the Backend first.)
* Term must be <= 30 years. Build a safeguard or input check to prevent users from entering data that is out of bounds. A message
box or modal is a good way to notify the user of an error.

# Submission

When you're ready to submit your work, zip the source directory
and email it to your HR contact. We're excited to see what you come up with!
