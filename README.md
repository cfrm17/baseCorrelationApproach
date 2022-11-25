# Base Correlation Approach


To our best knowledge to date, there is no generally accepted methodology of applying base correlation to a non-index CDO trade.  The proposed approach serves the purpose of finding an appropriate correlation to value a collateral debt obligation (CDO) tranche from market information via base correlations. The methodology involves four steps.

1.	Compute implied correlations and expected losses from the market prices of the standard index CDO tranches. 
2.	Build a base correlation curve for the index CDO trade.
3.	Build an “equivalent” base correlation curve for a bespoke non-index CDO trade.
4.	Calculate market implied correlation applied to a tranche of the bespoke trade.


The model provides five methods to match expected losses and “Loss Ratio” is the proposed one. Test results show that the methodology appears reasonable and has been implemented correctly. Therefore, the GCP base correlation template is approved, subject to following conditions:

1.	“Loss Ratio” is approved out of five methods. 
2.	The current model is not applicable to CDO2 and CDO3.
3.	The pricing models in the template, other than GCP Poisson model and Normal Copula model with fixed recovery rate, are not tested in this round and subject to future vetting.  


The approach implements a GCP base correlation, which is employed to calculate an appropriate correlation in the valuation of a collateral debt obligation (CDO) tranche using market information.  The credit default swaps (CDS) indexes iBoxx and Trac-X portfolios have been introduced in the market and the standard tranches linked to these reference sets are actively quoted. From this market information, we intend to retrieve the correlation information of the standardized collateral pool and then use it to get the market implied correlation for 1) non-standard CDO tranches with standard collateral pools and 2) tranches of a bespoke non-index CDO trade.

The base correlation curve is defined as the correlation inputs required for a series of equity tranches that gives the tranche value consistent with quoted spreads. It was introduced by JPMorgan to address the difficulty of applying market-implied correlation to a tranche with non-standard attachment and detachment points. 

However, as far as we know there is no established methodology to extend this approach to find the implied correlation of a non-index CDO trade, in which the writedown structure, collateral pools, and maturity could be quite different from the market quoted indexes. Some relevant research has tried to bridge the differences between the different standard indexes.

As shown below in this report, five mapping criteria have been implemented in the submitted GCP model. All of them somehow try to match normalized expected losses. It seems reasonable based on the assumption that CDO trades have a similar loss distribution function with a given level of correlation. Based on the testing, “Loss Ratio” is confirmed to be the most appropriate one, which is theoretically consistent to the algorithm of building base correlation curve and more robust for GCP Poisson model.

In the approach, the procedure involves four steps. The first step is to determine the implied correlation for each tranche of a specified index trade, which is named the “Source Trade” on the template.  From the implied correlation, we could get the implied expected loss of each tranche, denoted as  for ith tranche with attachment point and detachment point  . For example, the standard North America index is tranched into 6 tranches with  .  

Step two is to convert the expected loss of each tranche into a base correlation curve, by using the pricing model (see https://finpricing.com/lib/EqCallable.html ) in reverse to match expected loss function for a series of synthetically created equity tranches

(1)		 

Having worked out the base correlation curve for the reference trade, the remaining task is to value tranches of a bespoke trade, which is named “Target Trade” in the template. 

In the third step, an “equivalent” base correlation curve for the bespoke trade is calculated by scaling expected loss function of a series of equity tranches of the bespoke trade to that of the source trade. In other words, we intend to find a set of points for the target trade   which is “equivalent” to the   on the source base correlation curve. Five criteria for solving the equivalent tranches have been implemented, as shown in Table 1. 

Table 1. Five criteria of finding “equivalent” base correlation curve of the target trade
Criteria	Equations to solve 

Loss Ratio	 

Loss Fraction	 *

Breakeven Spread	 

Scale	 

Probability	 

* - Principal amount of  tranche of the target trade

The fourth step is to find the implied correlation of the tranche of the bespoke trade, after we get the “equivalent” base correlation curve. The breakeven spread is calculated by interpolating the base correlation curve using the equivalent tranche points as an index curve, re-price the target portfolio twice using the two values of correlation, and calculate the breakeven using the appropriate subtractions and divisions.

•	Benchmarking/Comparison to Other Models

As stated above, there isn’t a generally acceptable methodology for performing this mapping of non-index CDO tranche to the index market. Because “Loss Ratio” is the accepted method, the other four alternatives can then be viewed as benchmarking/comparison models.

Applying a based correlation curve to a non-index CDO trade by matching normalized expected loss, as indicated in the report, bears the assumption that CDO trades have a similar loss distribution function with a given level of correlation. This idea seems reasonable and is similar to the idea of the diversity score.  Five methods available in the submitted model are essentially different ways of normalizing expected losses. The “Loss Ratio” method is approved because it is consistent to the methodology base correlation curve generation, most robust in the presence of MC noise, and independent of pricing model to the largest extent among five methods.  

The current approach cannot be applied to CDO squared and cubed, although the submitted template has implemented them. The loss distribution function of CDO squared and cubed usually can be manipulated by the setup of the trade and could be quite different from that of a CDO trade. Hence theoretically it is not correct to apply current approach, which is achieved by matching normalized expected losses, to CDO squared and cubed. 

Several models, which are different from approved GCP Poisson model and Normal Copula model, have been implemented in the template, for example, a stochastic recovery rate model with beta distribution.  They are not tested this time and subject to future vetting, when GCP Phase II pricing engine is submitted.


