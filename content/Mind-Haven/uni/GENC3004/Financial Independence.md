---
title: Personal Finance
tags:
  - personal-finance
  - superannuation
  - super
  - FIRE
  - investments
cssclasses: 
author: Andrew Hingston
---
A situation in which the income you are drawing from your investments is enough to cover your living expenses.

## Financial Independence Retire Early (FIRE)
Every expense should be compared to time spent at work to earn the purchase
Keep living expenses very low and aim for 'extreme saving' of 70% of after-tax income
Once investments are 30 times living expenses (roughly 1 mil), you can quit your daily job and completely retire

### Variations on FIRE
FAT FIRE - More traditional lifestyle who saves more than the average retirement investor
LEAN FIRE - Minimalist living and extreme savings with a far more restricted lifestyle
BARISTA FIRE - Work casual job to cover some living expenses (so they don't erode investments too much)
COAST FIRE - Work part-time job but have enough saved to fund their current living expenses

## How much is needed
**ASFA Retirement Standard**
Annual living expenses assuming you are healthy and own your own home - so no rent/home loan repayments

|                       | Couple      | Single      |
| --------------------- | ----------- | ----------- |
| Comfortable lifestyle | $72,663 p.a | $51,630 p.a |
| Modest lifestyle      | $47,387 p.a | $32,915 p.a |

**FIRE Rule of Thumb**
Multiply by 30 to get investment required for financial independence

|                       | Couple     | Single     |
| --------------------- | ---------- | ---------- |
| Comfortable lifestyle | $2,179,890 | $1,547,900 |
| Modest lifestyle      | $1,421,610 | $976,450   |
![[Pasted image 20250105115730.png]]

### Perpetuity
Infinite stream of regular cash flows, the first cash flow occurs at the end of the first period (C_1), the cash flows can be level, growing or declining at a constant rate (g)
Useful for calculating the investment required to fund a perpetual stream of living expenses
**Present value of a growing perpetuity formula**
![[Pasted image 20250105123235.png]]
$P_{\infty,g} = \frac{C_1}{r - g}$
g < r
'C_1' = cash flow growing at a rate of 'g' per period
$\infty$ = they go on forever (with first cash flow at end of first period)
'r' = effective rate of return period (time value of money)
'P' = value of the infinite series of cash flows of 'C' at start (left)
'g' = both growth in income and investments
![[Pasted image 20250105120246.png]]

[[20250105#Financial Independence questions|Financial Independence questions]]

Growing perpetuity (inflation >= 0) -> more value of cash flow at the start
Decreasing perpetuity (inflation < 0) -> less value of cash flow needed at the start

**Present value of an annuity formula**
![[Pasted image 20250105123222.png]]
$P_n = C \times [\frac{1 - (1 + r)^{-n}}{r}]$
C = regular cash flows that are being moved to the left
n = number of cash flows (with first cash flow at end of first period)
r = effective rate of return per period
'P' = regular series of 'n' cash flows of 'C' at the start (left)

## How can I achieve it
1. Calculate financial independence goal - $PV of future living expenses at the financial independence target age
2. Calculate future value of existing savings - FV of single cash flow at target rate
3. Calculate financial independence gap - Financial independence goal from step 1 minus FV of existing savings from step 2
4. Calculate regular savings required - Future value annuity formula solving for 'payment' C with gap from step 3 as the target.

Example:
Current age: 30
Independence Age: 60
Future Expenses: $100,000 growing at 1% p.a. in real terms
Current Savings: $150,000 invested at 6% in real terms



Financial independence goal:
Calculating the amount required at age 60 ($P_{60}$)
To fund expenses starting at age 61* ($C_{61}$ = $100,000) growing at 1% p.a. assuming our investments achieve a real return of 6% p.a.
$P_{60,\infty,g} = \frac{C_{61}}{r- g}$
$= \frac{100,000}{0.06 - 0.01}$
=2,000,000

Future value of current savings
$F_60 = P \times (1 + r)^n$ = $150,000 \times (1 + 0.06)^{30}$ = $861,524

Calculate financial independence gap
$G_{60} = P_{60} - F_{60}$
=2,000,000 - 861,524
=1,138,476

Calculate regular savings required
Assuming 6% p.a. real return
$C = 1,138,476 / [\frac{1.06^{30} - 1}{0.06}]$
=14,400 p.a.

## What are some common strategies
**3 Key Principles**
1. Consistently 'save to invest' at least 20% of your income (before-tax)
2. Avoid dumb investment decisions
3. Diversity across multiple financial strategies
#### Invest in your ability to generate income
Invest time into your career plan
Invest in ongoing work-relevant education
Invest in physical and mental health

#### Invest in a minimum of one property
Controls dwelling expenses over the long term - rent increases with average income and inflation - but mortgage payments do not
Expected price growth linked to growth in average income - real price growth 2.5% + inflation 2.5Z% = nominal price growth 5.0% per annum. You also save on paying rent of about 3% of property price
May only need a 2 bedroom apartment or small house when you are 60+ - benefits of downsizing from large house and investing the difference 
Property can be used as security on investment loans - why gives you the option to invest in other asserts (such as property* or shares)

#### Invest in a diversified portfolio of shares
Provides growth over long-term - regular income through dividends
Expected price growth linked to average income - Real price growth 2.5% + inflation 2.5% = nominal price growth 5.0% p.a., you can also receive dividends average about 3.5% p.a
Invest in ETF\
Minimum investment time horizon is 5 years
Avoid short-term speculations

#### Use retirement savings effectively
Retirement savings systems have tax advantages
Concessional (low) tax on money going into the system
Concessional (low) tax on investment returns
Tax-free income streams after the age of 60 in Australia

#### Get involved in a start-up 
Start-ups can generate millions of dollars of wealth in just a few years
Provide valuable business lessons whether succeed or fail
Avoid investing a significant amount of your own money

Don't rely on only one or two strategies
Best to diversify between three/four strategies

## Superannuation system
Retirement savings system in Australia

Cannot access until you turn 60 (or it may change in the future)
Employer makes regular contribution - those contribution are usually taxed at 15% rather than marginal income tax rate + medicare levy (34.5%)
which are investment and accumulate over 30+ years at which point you start drawing a regular income from your super

**Two types of systems**:
*Accumulation (contribution)*
Account balance is sum of past contributions + returns
Retirement income depends on account balance
Risk of underfunded retirement is borne by the individual
Risk of default is very low

*Defined Benefit (phased out of Australia)*
Retirement income depends on salary in final years before retirement
Guaranteed income stream until death (with partial reversion to spouse)
Risk of underfunded retirement is borne by the company (or the gov)
Risk of default is medium to high (often needs to be underwritten by government)
![[Pasted image 20250105134647.png]]

There is a limit to how much you can accumulate in super and still receive concessional tax treatement
the limit is called the 'transfer balance cap' - lifetime limit that can be transferred from accumulation phase to retirement income phase
Transfer balance cap: $1.9 million per person - indexed in $100,000 increments every few years in line with CPI. This is 'per individual' so a couple can accumulate double this if equally balance.
Anything above this is called 'Excess Transfer' - these can be subject to an excess transfer balance tax of 15% (first) to 30% (second +)

#### Types of funds
1. MySuper - Low fees, simple features, few investment options. Offered by retail and industry funds
2. Industry super funds - Low to medium fees offered by not-for-profit organisations (some linked to unions)
3. Retail super funds - Medium to high fees offered by financial institutions. More investment choices
4. Public sector super funds - For government employees
5. Corporate super funds - large companies such as Telstra, Qantas..
6. Wrap accounts - Popular with financial advisers as they offer sophisticated investment options
7. Self-managed super funds - Fund set up for a family that maintains direct control over assets. High accounting fees

#### Life insurance within super
Death cover - lump-sum payment upon death of member
total and permanent disability (TPD) - lump-sum payment upon total and permanent disability
Salary continuance (income protection) - replaces part of incomes if accident or illness prevents you from working

Insurance premiums cheaper within super
Need to be careful don't lose benefits when changing funds
Premiums can be a drag on achieving long-term goals

#### Accessing super
Prevent age currently is 60
Excemption:
	first home super saver scheme
	compassionate grounds
	financial hardship
	emergency release programs

#### THE GOOD
1. Regular savings paid for you by employer
2. Concessional (low) tax on money going into super
3. Concessional (low) tax on investment returns while inside super
4. Concessional (low) tax when drawing income from super
5. Good investment options with good returns
6. You normally can't access your super until at least age 60
7. The AUS gov supports the system

#### THE BAD
1. Super rules are quite complicated and confusing
2. Many different super funds resulting in 'choice anxiety'
3. Easy to accumulate multiple super funds with changing jobs
4. Some super funds have high fees and poor returns
5. Some funds give you default life insurance when you don't need it
6. You normally can't access your super until 60
7. Government keeps changing the rules

### Consolidating your super
==Only have 1 account in total==

#### Super stapling
'Stapled' to the first fund you sign up with or the fund you are with now
So when moving jobs your super fund will 'move' with you - unless you choose different super fund

You are not 'stuck' with the same super fund, you can change fund whenever you like

**What jobs may not have paid super**
under 18 and < 30 hours of work per week
'domestic worker' and < 30 hours of work pw
self-employed

### Investment options
#### Three layers
![[Pasted image 20250105142902.png]]
**Investment alternatives**
1. Investment options - super fund chooses both asset allocation and managed funds
	You can choose between 'conservative', 'balanced', 'growth'
	low fees but reduced choice and flexibility
2. Assert allocation - You can choose asset allocation and super fund chooses the fund managers
	you choose allocation between cash, fixed interest, shares, listed property, ....
	slightly higher fees in exchange for more choice and flexibility
3. Direct investment - can choose both asset allocation and fun managers (something specific shares)
	can choose specific manage funds (or shares) from a large menu of options
	Higher fees in exchange for maximum choice and flexibility

Better to choose higher total risk below the age of 50 and then gradually reduce investment risk levels after that

#### Wrap accounts
such as Macquarie wrap
Provides many investment choices - allowing tailored asset allocation and selection of assets to suit the client
adviser fees can be deducted directly from investment returns, easy administration and reporting across many clients
higher admin fees
### Contributions to super
![[Pasted image 20250105143558.png]]
![[Pasted image 20250105143604.png]]
#### Limits for concessional contributions
These are 'before income-tax ' contributions
	including employer super guarantee contributions or salary sacrifice contributions
	Contributions tax rate is 15% rather than paying marginal tax rate plus Medicare Levy
Concessional Contribution Cap (maximum): $30,000 per year
	any contribution above the cap taxed at marginal income tax rate plus Medicare Levy 
	can carry-forward unused cap over 5 years rolling period if super balance < $500k
High income earners may pay an additional Division 293 tax of 15%
	the division 293 threshold is $250,000 p.a. based on combined income and contributions
	this is till lowers than the highest marginal tax rate of 47% (including medicare levy)
personal after-tax contributions (with deductions) limited if aged 67+
	age 67 - 74: must work 40 hours in consecutive 30 day period. Age 75+: not allowed
	employer and salary sacrifice contributions still allowed until age 75 with no work test

#### Limit for non-concessional contributions
these are 'after income tax' contributions made from bank account
	including funds released by selling other investments and moving the funds into super
Super account balance must be below Transfer balance cap ($1.9 million)
Non-concessional contribution Cap(maximum): $120,000 p.a.
Age restriction:
	Age < 75: able to 'bring forward' 2 years of caps and contribute up to $360,000
	Age >= 75: not allowed

#### Ways to get money into super

*Employer contributions (including super guarantee contributions*
	Employer contribute percentage of ordinary time earnings into super
	Minimum requirements are Superannuation Guarantee Contributions (SGC)
	Most employers required to allow you to choose funds
	Concessional contributions subject to 15% contributions tax
*Self-employed contributions*
	Need to make their own concessional contributions themselves
		1. send a 'notice of intent to claim' form to your super fund before EOFY
		2. make a personal contribution directly to the superannuation fund from bank account
		3. claim a tax deduction for the contribution in your tax return to the ATO
*Salary sacrifice contributions*
	Asking employer to pay part of your salary to super (15% tax)
	Concessional contributions, popular with people approaching retirement (50 - 74)
	Salary sacrificing can affect terminal benefits under some contracts
*Personal after-tax contributions (with deduction)*
	Payments direct from bank acc to super for which you do claim a tax deduction in your income tax return (15%)
	concessional contributions subject to cap ($30,000 p.a.)
	popular way to increase super and reduce income tax
	must notify your super fund of your intent to claim a deduction and receive (NAT71121 Notice of intent to claim or vary deduction for personal contributions)
	Usually eligible for FHSS scheme
	Not eligible for Government Co-Contribution
*Personal after-tax contributions (with no deduction)*
	Payments direct from bank acc to super for which you do not claim a tax deduction in your income tax return
	non-concessional contribution subject to cap ($120,000)
	popular with people approaching retirement (selling investment property, and invest proceeds into super)
	eligible for fhss scheme
*Government co-contributions*
	Gov can top-up $0.5 for each $1 (up to $500) of personal after-tax contributions (up to $1000)
	income must be below $45,000 (2024-2025) to get the full amount 
	ATO and super fund arrange this automatically
	possible strategy for spouses or uni students with casual work
*Spouse contributions*
	Superannuation contribution on behalf of a low-income spouse
		Sum of spouse's assessable income, fringe benefits and super < $40,000
		Spouse can be married or defacto and must be living together
		Both you and spouse are aus residents
	Non concessional contribution (after-tax)
	contributor is eligible for tax rebate (max $540)
*Child contributions*
	Infants and children can have superannuation accounts - some superannuation funds will not permit infant accounts due to inability to sign forms
	adults can make contributions into the child or infant's accounts
	non-concessional after-tax contributions
		no tax deductions can be claimed by the adults making the contributions 
*Downsizer contributions*
	contribute up to $300,000 from the sale of your home into superannuation
	can only be access once by people aged 55 or over
	an after-tax contribution made into superannuation
	neither a non-concessional or concessional contribution
*Sale of small business*
	rollover proceeds from sale of small business into superannuation and defer tax on up to $500,000 in capital gains
![[Pasted image 20250105150313.png]]

### Income from super

**Minimum pension payments based on age**
![[Pasted image 20250105151816.png]]

#### Transition to retirement (TTR) pension
Can access an income stream from super while you are still working
Must reach the 'prevention age'
Draw minimum of 4% each year if under age 65
Lump-sum withdrawals not allowed unless you permanently retire

Pension income normally tax-free
investment earnings subject to 15% tax rate

#### Account based pension (ABP)
allows you to draw a regular income from super savings once you have reached preservation age and permanently retired
can still make lump-sum withdrawals

60+ income received in tax-free
investment earnings are tax-free
lump-sum withdrawals are usually tax-free

#### Annuity
Pays regular income regardless of market returns - income can be linked to inflation
term can be fixed or lifetime
limited or no access to lump sum withdrawals

60+ - tax free
investment earnings not relevant
![[Pasted image 20250105152203.png]]
### Withdrawing from super

**Why?**
Still have home loan upon retirement
No Trust in government
No Trust in Super funds
Health expenses
Retiring overseas
Helping adult children reduce their debt
Helping grandchildren buy their first property
Lavish family holiday

#### Types of withdrawal
*After age 60*
	Withdrawals are often largely tax-free but need to satisfy 'condition of release'
	if you invest the funds outside the super environment then investment returns are assessable at marginal tax rates
*Economic crises*
	Things like COVID-19
	Withdrawals were tax free
	Provides emergency financial slack
*FHSS*
	Withdraw up to $50,000 to help buy first home (only access up to $15,000 of contributions per year)
	Assessed individually so couples can withdraw $100,000
	Must be either:
	1. Salary sacrifice contributions (concessional)
	2. Personal after-tax contribution with deduction (concessional)
	3. Personal after-tax contribution with no deduction (concessional)
	not employer contributions
	withdrawals of concessional contributions for FHSS to buy first home
		pay marginal income tax rate less 30% tax offset when withdrawn
		save roughly 15% tax on savings channelled via super to buy first home
*Compassionate grounds*
	medical treatment/transport for you/dependent
	palliative care for you or dependent
	making payment on home loan or council rates so you don't lose home
	accommodating a disability for you or your dependent
	expenses associated with the death, funeral, or burial of dpendent
	tax is payable on the withdrawals
 *Severe financial hardship*
	 meet all of the following:
		 unable to pay essential family living costs
		 receiving income support payment for at least 26 weeks in a row
*Terminal medical condition*
	Certified by two registered medical practitioners
	Illness likely to result in death within 2 years
*Incapacity* 
	Temporary incapacity - unable to work due to physical/mental medical condition
	Permanent incapacity - permanent physical or mental medical condition
*Super less than $200*
	employment terminated and balance is below $200
*Departing Australia*
	Temp visa holders can withdraw super upon leaving Australia. DASP ordinary tax rate is 35%, contributions tax of 15% was also paid
*Divorce*
	Super assets are included in 'asset pool' distributed in a divorce
	Family course can order super funds to pay part of super to ex-spouse
*Death*
	
### Super Strategies

**Make additional contributions into super**
Salary sacrifice at least 10% of your salary or make a personal after-tax contributions and claim a tax reduction, take advantage of long-term compounding of returns
Say under the Concessional Contribution Cap ($30,000 p.a.) Employer contributions included in this cap

**First Home Super Saver (FHSS)**
Withdraw salary sacrifice or personal contributions to help buy first home
$50,000 per individual ($100,000 for couple)

**Invest in high-growth investment option**
While under 50 consider investing in high-growth option
Higher volatility (risk) in exchange for higher expected average return
Mainly Australian and international shares

**Children's super**
Open super account for each child 
Contributions should be made from a bank account in child's name

**Increase contributions once you reach 45**
From age 45+ - try to salary sacrifice more or make personal after-tax contributions and claim a deduction
Reduces income tax payable

**Self managed super fund (SMSFs)**
Creating your own fund with 6 or fewer members
Direct control over investments with more choice (including property)
Better control over tax when moving from accumulation to payment phase
Borrow to invest using limited recourse borrowing arrange (LRBA)
Ability to transfer residual amounts to other fund members upon death
... but pretty high admin costs and an accountant will need to be involved

**Super balancing**
Attempt to equalise account balances for you and spouse
spouse contributions
Super splitting
	an individual can split up to 85% of concessional contributions to a spouse
	Receiving spouse must be under the age of 65
	Non-concessional contributions cannot be split
Personal after-tax contributions
	Sale of assets outside super - direct into spouse's super account
Helps avoid breaching transfer balance cap

**Transition to Retirement (TTR)**
Continue working part-time after age of 60 as long as possible - allow your super balance to continue growing
Salary sacrifice income above $45,000 into superannuation - pay 15% contributions tax rather than 30% marginal income tax (plus Medi levy). Subject to concessional cap of $30,000 p.a. (but can be higher with unused cap)
draw tax-free income from TTR pension to help cover living expenses
Net effect is to lower marginal tax rate to 15% + 2% Medi levy

**Sell investments outside super**
Before retirement - Simplify your investments by selling properties or shares outside super
Invest proceeds into super as personal after-tax contributions
Watch out for non-concessional contributions cap (120,000 p.a.)
Watch out for timing of capital gains on sale of assets - best to sell those assets in a financial year in which you are earning little other income

**Downsizing home after age 60**
Downsizing the family home to a smaller one - must have lived in the home for 10 years prior to sale
downsizing contribution of $300,000 into super - for a couple this is double, watch out for transfer balance cap (1.9).

**Age pension**
The regular income can help your super to last a lot longer

**Enjoy life**
avoid complex super arrangements, tax structures and investments
Draw a regular income
Keep next 5 years of pension payments in low-risk (fixed interest)
Keep the remainder in growth investments (shares and listed property) - preferably use ETFs to avoid gambling