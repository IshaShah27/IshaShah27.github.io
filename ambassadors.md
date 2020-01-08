## U.S. ambassador expertise by host country regime type
*How do the qualifications of U.S. ambassadors differ by the regime type of their host country?*

U.S. ambassadors largely come in two flavors: career and political appointees. Career diplomats have experience as Foreign Service Officers (FSOs) or have served in some other professional government capacity in the country or region to which they have been appointed ambassador. Political appointees are supporters of the president do not have experience working in the State Department, although they are likely to be prominent in other domains: recent political appointees have included hoteliers and chiropractors. The practice of appointing an average of 30 percent of the nation's highest-ranking diplomats based on their political support of the president is ["aberrational among advanced democracies" according to Ryan Scoville, a scholar studying ambassadorial qualifications](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3333988).

Scoville found that political appointees were less likely to speak the language of the host country, less likely to have previous experience in the country or region, and more likely to have donated money to the presidential campaign. He released an impressive dataset hard-won through FOIA requests of the qualifications of each U.S. ambassador from the Regean administration through the first two years of Donald Trump's presidency. Using this dataset and a [dataset created by political scientists Boix, Miller, and Rosato](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/FJLMKT) that codes the regime in place in each sovereign country in each year from 1800-2015 as either democratic or non-democratic, I took a look into whether there have been differences in the types of appointees to democratic and non-democratic countries. 

Although the outcome of this exercise is likely intuitive, like Scoville's results, the quantification of what is qualitatively apparent can be useful.  

**Question:** Is there a connection between the type of ambassador that is appointed to a country (either career or political) and its regime type (i.e., are nations with autocratic regimes more likely to have career foreign service officers as ambassadors, perhaps because they require more advanced expertise to maintain relations)?
    
**The current hypotheses:** I expect that non-democratic countries will have ambassadors who are more likely to be career diplomats, have familiarity with the language (despite fewer of these countries using a commonly-learned language in the U.S.), and have experience in the region. I also expect that the share of political vs career appointees for non-democratic countries will not vary much across presidents, unilke the overall share. Over time, I expect that an increasing share of ambassadors to these non-democratic countries will be career, rather than political appointees.
  
**Questions for future analysis:** In the future, I would be interested in doing a simple regression analysis - perhaps a logit model where the dependent variable is the likelihood of being a political appointee, and the independent variables include regime type, campaign donations, familiarity with language, and regional experience. It might be interesting to do this by president or political party to see whether the coefficients change.

**The datasets:**
1. *Ambassador qualifications:* The primary dataset is of the qualifications of each U.S. ambassador appointed from 1981 to 2018, where each row describes an individual appointee-year-country combination.
- The variable for each appointee that is most relevant to my analysis is whether the appointee is a career foreign service officer. I also plan to use variables indicating whether they can speak the principal language of the country to which they have been appointed and whether they have experience in the host region in future analyses.
- Other variables such as year of appointment and host country will be relevant for joining the primary and secondary datasets.
- The dataset was assembled by Ryan Scoville, a researcher on international law at Marquette University Law School and a writer for the Lawfare blog.
2. *Regime type:* The secondary dataset that represents whether a country is a democracy in a given year, for all sovereign countries for each year from 1800-2015. It also states whether the country was in democratic transition transition, the years it had held its democratic or non-democratic status, and whether women had the right to vote.
- Each row in the dataset represents a country in a single year, for each year from 1800 to 2015.
- The variable that is most relevant to my analysis is "democracy," which indicates whether that nation had a democratic government in a given year
- The source of the dataset is a group of researchers (Carles Boix, Michael Miller, and Sebastian Rosato) at Princeton University, Australian National University, and the University of Notre Dame, respectively, who introduced it in their paper in Comparative Politics "A Complete Dataset of Political Regimes, 1800-2007."

### Data cleaning 
The most laborious part of this exercise was adjusting country names between the two datastes to allow for a match of the qualifications of an ambassador and the regime type of their host country in the year in which they were appointed. The assumptions I made in this process were minor:
- The regime type of the host country in the year of the appointment is most relevant to the appointment decision, (earlier years are probably also important, but I assumed not as much as the appointment year)
- For Brunei, Kosovo, Kiribati, the Marshall Islands, and Micronesia, there were several years in which they did not appear in the regime type dataset; however, after some digging into the histories of each of these countries it seemed to me that all of them had the same regime type in these years as they did in the first year they appeared in the dataset. I applied the regime type in their first chronological record to previous years.
- For Serbia and Yugoslavia, my unfamiliarity with the region and lack of international political consciousness in 2001 meant that I could not reconcile the ambassadors dataset, which had one appointee to each country, and the regime type dataset, which only had an entry for "Serbia and Montenegro" in this year. I dropped this record.
- I also dropped the Holy See, since it did not appear at all in the regime type dataset.

### Aggregating

After cleaning the data, we're off to the races!

### 1. Differences in language skill and regional experience between career and political appointees
  
In order to first make sure that the differences Scoville noticed between career and political appointees are statistically significant, I used a simple t-test to compare the mean share of political/career appointees who had (1) proficiency in the principal language of their host country and (2) experience in the region.
  
Both are statistically significant, with a p-value less than 0.01.  
<img src = "images/sigtest_fso.png?raw=true/">

### 2. Differences in likelihood of being an FSO, knowing language, and having regional experience by regime type
  
Then, I tested my own hypothesis - that because non-democratic regimes may be more sensitive to ambassadorial conduct, that the ambassadors to them are less likely to be political appointees. It turns out that we fail to reject the null hypothesis, which is that there is no difference (in these characteristics) between ambassadors appointed to democratic vs non-democratic countries. In other words, it is the case that a U.S. ambassador appointed to a non-democratic country is more likely to be a career diplomat, more likely to know the principal language, and more likely to have experience in the region. These differences are also significant at the 1% level.
  
<img src = "images/ambdiff_regtype1.png?raw=true/">
<img src = "images/ambdiff_regtype2.png?raw=true/">

### 3. Differences in likelihood of appointing a career diplomat, by regime type and president
  
Finally, I thought it would be interesting to see whether certain administrations were more or less likely to have these differences in appointment type for countries of different regimes. My hypothesis is that while there is probably at least some variation in the overall share of appointees that are career FSOs between presidents, that these differences will not be significant. However, I do think we will see a significant effect when looking at democracies alone - when considering the posts that from background knowledge I know are the most likely to have political appointees (e.g., the U.K., France, Liechtenstein), they are almost all in advanced democracies. I think that there will be much less variance in likelihood for non-democratic countries, which may be masking the differences by administration in the democracies.
  
<img src = "images/careerfso_bypres1.png?raw=true/">
  
It looks like there are slight differences between in the share of appointees that are career FSOs between presidents, but it doesn't seem that the differences are all that large. I did an ANOVA test to check if this share differed between administrations:
  
<img src = "images/overall_presfso.png?raw=true/">
  
It looks like it didn't. The prob(F-statistic) = 0.271, above any p-value threshold. However, since we've seen that the difference in share of appointees that are career FSOs are different between regime types, it's possible that a single regime type does have a significantly different share of appointee types, and the difference is masked when democracies and non-democracies are combined. So, I did an ANOVA by regime type:

<img src = "images/demo_presfso.png?raw=true/">
  
<img src = "images/nondemo_presfso.png?raw=true/">
  
It looks from these that actually aren't significant differences between any of the presidents, although the model for democracies-only is significant at the 0.05 level.
  
### Conclusions

**TLDR;** An analysis of ambassadorial qualifications by regime type of host country (democracy or non-democracy) suggests:
- Ambsassadors to non-democracies are more likely to be career diplomats, speak the principal language of the country, and have experience in the host region
- While the overall share of career versus political appointees (irrespective of regime type) appeared to vary between the administrations of Ronald Reagan, George W. Bush, Bill Clinton, Obama, these differences did not reach the significance threshold. All administrations had a greater share of career diplomats appointed to non-democracies than democracies.
- When looking only at democracies, there does appear to be a difference in the share of ambassadors who are career diplomats between administrations.
- A closer look into how this practice evolved over time, and how the current president's appointments compare to those of his predecessors would be interesting next steps.
- [If interested, you can find the full notebook here. Thanks for reading!](https://colab.research.google.com/drive/1daAu0oMVEL6bbEsckvhYzbDwyDXIR1PZ).
