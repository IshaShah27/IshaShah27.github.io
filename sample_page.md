## Jim Comey, Senate Intelligence Committee, June 2017: A test site for NLP techniques
*What can Jim Comey's testimony tell us about using natural language processing techniques to analyze court proceedings?*

Contextualization is more difficult for machines than humans. As current ML techniques stand, this is especially true in tasks related to understanding natural language. Full of idioms and turns of phrase, individual eccentricities, and complicated tones, it can prove difficult for any of us, machine or human, to understand what others are saying.  
  
The halls of the U.S. Congress, for example, are home to proceedings that often flummox both human and machine interpreters. While humans are able to easily understand what the members of the House or Senate committees and the respondents are saying, it can be difficult to assess what they *mean*. Computers, with a perhaps more rudimentary grasp of tone and expression, face an even greater challenge. Throw in the soup of motivations of all parties bubbles in the background of these questionings and the verbal contortions that respondents often make, and it seems truly impossible that, on the surface, natural language processing techniques can tell us anything about these proceedings that the hundreds of politicos, journalists, and talking heads across the country who are paid to perform the very task of interpreting the testimony cannot. And yet, applying these techniques in this context is *extremely* tempting:  

  - For one, **most congressional testimonies are important and influential**. Even if there is a marginal improvement in our understanding of these proceedings, it could have large a large impact. 
- Transcripts are legally required to be made and released to the public after an open testimony, providing a **clean, high-quality, and easily processed** dataset that is almost unparalleled in its ease of access.
- Focusing on only text as data, rather than visual cues, might allow for analysts to understand testimonies in a way that is **free of visual bias** stemming from one’s appearance and inferred demographics, whether related to age, race, gender, or any other visual presentation.
- NLP techniques allow for the simultaneous **processing of large volumes of testimonies at once**, allowing analysis of a corpus of data whose size would be prohibitive for qualitative research.
- NLP techniques also present a **unified framework** that can be applied across different testimonies. These frameworks could be used to compare performances by senators or representatives or testifiers across hearings, or compare how hearings on the same subject are distinctive depending on who the senators or representatives are.

To me, it seems like applying NLP techniques to transcripts of court proceedings could open up a whole new realm of analytical possibility. And yet, it is clear that they might have to be applied awkwardly, and that there may be some trial-and-error in finding the questions these techniques can and cannot be used to answer. So, I went for it.  
  
  In order to explore some of the potential uses and pitfalls of applying NLP techniques to congressional testimonies, I used former FBI Chief James Comey's testimony before the Senate Intelligence Committee on June 8th, 2017. This testimony in particular provides a particularly rich corpus on which to test NLP techniques, because James Comey's political position in June 2017 was fascinatingly complicated. The purpose of the Congressional hearing was to learn about the circumstances of Comey's firing from his role as FBI Chief, which Comey suggested was due to his failure to comply with President Trump's requests for his "loyalty" in matters related to the Russia investigation, rather than his job performance.
    
**The current hypotheses:**  
- Republican senators may treat Comey more favorably than their Democratic counterparts. Comey has been vocal in confirming that the president is not under investigation and has also served under other Republican presidents as attorney general, making him perhaps a sympathetic figure within the Republican party. Democrats may be more willing to take a more aggressive tack toward his questioning - many may believe that it was Comey’s announcement of the re-opening of an investigation against Hillary Clinton, the Democratic presidential candidate, that led to the party’s loss in the 2016 election. *(Method: sentiment analysis)*
- The possibility that there is a distinction in the topic and tone of questioning between senators of different parties also introduces the opportunity to do a supervised learning exercise in order to identify the party of the questioner. If a well-tuned interpretable machine learning model can predict the party of a questioner above the level of chance, it could reveal distinctions between parties and senators that are not apparent from qualitative analysis. *(Method: supervised classification)*  
  
**Questions for future analysis:**  
- Which topics do Republican and Democratic senators focus on? The answers yielded by an NLP analysis would be useful to compare to those of human readers, to determine if topic modeling techniques can parse this sort of testimony well enough to be applied to large numbers of testimonies at once. While human interpretations may be more accurate for a single testimony, research that requires reading through volumes, perhaps for a historical analysis, could be helped tremendously by machine learning techniques - if they perform well enough.
- Each of the senators on the Select Intelligence Committee has taken part in many other hearings, as has Jim Comey. How do their performances compare across these different contexts?

### 0. Pre-processing  
I removed a standard set of stopwords from the ```NLTK``` package, plus a few context-specific phrases that don't convey additional information: "ever said", "attorney general", and "Mr Comey" among them. I also lemmatized the text (reduced words down to root, so that "ran" and "running" would be recorded as the same word, as would "was" and "been").  

**Some notes on pre-processing:**  
- *Question-answer relationship*: In this analysis, I considered each question and response independently. However, sentiment and topic analyses that take the relationship into account could be useful. For example, does the sentiment of the question-answerer match that of the question-asker? Does one vary while the other remains constant? Are questions on certain topics more or less inclined to evoke sentiment-laden responses from the answerer?
- *Quote gremlins*: Often, when a senator asks a question of Comey, he or she would quote a part of his written record of his meeting with President Trump or the president's tweets. That a senator chooses to quote a certain conversation or document, or even chooses to quote at all, is valuable information - but more careful pre-processing is required to remove these instances where they interfere with the analysis (i.e., sentiment analysis), and include them where they are additive (topic modeling, perhaps).
- *Introductory remarks*: For the purpose of this analysis, which was to determine the sentiment and style of the questioning portion of the proceedings, I did not include introductory remarks. These remarks are highly illustrative - senators often state directly their relationship with the person on the stand, and their opinion about the proceedings as a whole. Can these remarks perhaps serve as a check to an analysis of the questions, or provide useful context? Is excluding them willfully ignoring important background knowledge, or does an analysis of questions alone useful precisely because it lacks this information?

### 1. Word clouds

Without further ado, word clouds:

*Democratic senators*  
<img src="images/rwc.png?raw=true"/>
  
*Republican senators*  
<img src="images/dwc.png?raw=true"/>
  
*Comey*  
<img src="images/comey.png?raw=true"/>

**A few quick observations:**  
- While senators of both parties feature the word investigation quite prominently, there seems to be a much greater diversity of words that appear with some frequency in the Republican word cloud than the one for Democrats. This could mean that when Democratic senators questioned Comey, they tended to make specific references to a limited set of discrete topics: “Flynn” (the investigation of Michael Flynn, a former Trump campaign aide), “dinner”  and “meeting” (the dinner Trump and Comey had together in which Comey claims the president asked for his loyalty, and “session” and “recusal” (in reference to Trump’s dissatisfaction with the recusal of Jeff Sessions, attorney general, from the Russia investigation).
- In contrast, the smaller size and greater number of the words in the Republican word cloud suggest that Republicans were perhaps more general in their questioning style and broader in the topics they covered – the same terms that are prominent in the Democratic word cloud highlighted above are much less so in the Republican word cloud, and instead words like “thing,” “conversation” (rather than “meeting” or “dinner”) appear. There are also fewer proper nouns in the Republican word cloud than the Democratic one, suggesting that their questioning may not have been quite as specific.
- In clear contrast to either of the senators’ word clouds, Comey’s contains considerably more words that suggest uncertainty, and they are featured prominently: “don’t know”, “something”, “well”, “thing”, “hope”, “might”, “don’t remember". This is also likely the case because Comey is more likely to convey how he felt or remembered his event – in other words, personally experienced it, rather than reporting an account that is as clear as that of a third-party observer. 
- 	In future analyses of congressional testimonies, particularly those where Comey or the same senators are involved, it would be interesting to see whether the differences in word frequencies seen in this example persist. For example, are these Democratic senators always more likely to ask questions about specific people and events to a greater extent than the Republican senators, or would it change depending on the political nature of the testifier or the subject matter the hearing was on? It would also be interesting to see whether James Comey’s responses are similarly indicative of uncertainty and personal experience in other testimonies compared to this one, which was about events directly related to the circumstances of his firing.

### 2. Sentiment analysis  

*AFINN sentiment score:*  

| Group      | AFINN average score |
|------------|---------------------|
| Comey      | 0.145               |
| Democrat   | 0.028               |
| Republican | 0.069               |
  
*NRC Emotion Lexicon:*  

| Group      | Anger | Fear  | Disgust |
|------------|-------|-------|---------|
| Comey      | 0.011 | 0.027 | 0.010   |
| Democrat   | 0.014 | 0.024 | 0.011   |
| Republican | 0.023 | 0.036 | 0.016   |
  
*NRC VAD (Valence-Arousal-Dominance) Lexicon:*  

| Group      | Valence | Dominance | Arousal |
|------------|---------|-----------|---------|
| Comey      | 0.397   | 0.256     | 0.377   |
| Democrat   | 0.438   | 0.328     | 0.424   |
| Republican | 0.427   | 0.307     | 0.402   |
  
*By senator:*  

| Name      | Group       | AFINN average | VADER negative | VADER neutral | VADER positive | VADER compound | NRC valence | NRC dominance | NRC arousal | NRC anger | NRC fear | NRC disgust |
|-----------|-------------|---------------|----------------|---------------|----------------|----------------|-------------|---------------|-------------|-----------|----------|-------------|
| BURR      | Republican  | -0.029180     | 0.079429       | 0.884179      | 0.036357       | -0.080821      | 0.427806    | 0.350429      | 0.444425    | 0.038514  | 0.078156 | 0.014041    |
| HEINRICH  | Democrat    | -0.058050     | 0.063211       | 0.894421      | 0.042368       | -0.017905      | 0.448911    | 0.340748      | 0.419049    | 0.038476  | 0.053475 | 0.026070    |
| REED      | Democrat    | -0.026029     | 0.092222       | 0.817667      | 0.090111       | 0.013244       | 0.417732    | 0.335338      | 0.428987    | 0.039870  | 0.064507 | 0.034229    |
| HARRIS    | Democrat    | 0.017519      | 0.017625       | 0.950708      | 0.031625       | 0.052987       | 0.412458    | 0.302013      | 0.430358    | 0.008356  | 0.018241 | 0.003148    |
| BLUNT     | Republican  | 0.077490      | 0.046000       | 0.888211      | 0.065842       | 0.076574       | 0.448405    | 0.316074      | 0.389958    | 0.010267  | 0.010267 | 0.005482    |
| COTTON    | Republican  | -0.009618     | 0.025188       | 0.917625      | 0.057188       | 0.097394       | 0.448356    | 0.333856      | 0.427471    | 0.030613  | 0.036585 | 0.020146    |
| McCAIN    | Republican  | 0.005999      | 0.031000       | 0.915533      | 0.053600       | 0.105340       | 0.419234    | 0.320158      | 0.415844    | 0.038079  | 0.050077 | 0.015285    |
| LANKFORD  | Republican  | 0.096441      | 0.031727       | 0.882136      | 0.086182       | 0.118650       | 0.441156    | 0.286160      | 0.394266    | 0.005437  | 0.021441 | 0.005437    |
| MANCHIN   | Democrat    | 0.087596      | 0.032111       | 0.898778      | 0.069167       | 0.121278       | 0.473201    | 0.354438      | 0.421901    | 0.004274  | 0.019530 | 0.011051    |
| COLLINS   | Republican  | 0.028660      | 0.015462       | 0.943000      | 0.041538       | 0.123085       | 0.361233    | 0.285425      | 0.355060    | 0.029102  | 0.028378 | 0.036543    |
| COMEY     | Comey       | 0.144648      | 0.116000       | 0.708101      | 0.175903       | 0.127569       | 0.397106    | 0.256403      | 0.377328    | 0.010965  | 0.026652 | 0.009728    |
| WYDEN     | Democrat    | -0.028911     | 0.030333       | 0.911000      | 0.058667       | 0.143733       | 0.436942    | 0.323478      | 0.441666    | 0.010401  | 0.012573 | 0.009769    |
| KING      | Independent | 0.079966      | 0.022000       | 0.882850      | 0.095150       | 0.186295       | 0.425925    | 0.304458      | 0.402361    | 0.002632  | 0.025057 | 0.000000    |
| RISCH     | Republican  | 0.288012      | 0.037882       | 0.822294      | 0.139941       | 0.238606       | 0.512447    | 0.294212      | 0.443826    | 0.016084  | 0.036211 | 0.009772    |
| FEINSTEIN | Democrat    | 0.183405      | 0.034000       | 0.778636      | 0.187364       | 0.277973       | 0.390371    | 0.300283      | 0.383555    | 0.004545  | 0.004545 | 0.000000    |
| RUBIO     | Republican  | 0.085778      | 0.014739       | 0.900870      | 0.084348       | 0.321561       | 0.352373    | 0.244928      | 0.324372    | 0.017393  | 0.018686 | 0.025193    |
| CORNYN    | Republican  | 0.113444      | 0.039600       | 0.806933      | 0.153600       | 0.412653       | 0.428579    | 0.324415      | 0.412504    | 0.027643  | 0.031230 | 0.021405    |
| WARNER    | Democrat    | 0.040866      | 0.021176       | 0.907765      | 0.071059       | 0.419341       | 0.467203    | 0.338793      | 0.434790    | 0.000000  | 0.002674 | 0.000000    |


### 3. Classification

Intro  

The corpus was then vectorized using the Term Frequency – Inverse Document Frequency measure, because the goal was to highlight differences between the two parties. I also used both unigrams and bigrams, since certain phrases like “Russia investigation” may make more sense unified than apart. I did not stipulate a minimum or maximum document frequency, because the number of questioners and the number of statements were both relatively small, and the use of rare words or phrases by senators is relevant in this context.  

Since the classification was intended to answer the question of whether senators could be distinguished by their questions, I dropped Comey’s statements from the dataset, leaving 168 statements by Republicans, 110 statements by Democrats, and 20 statements by the Independent. I used an 80-20 training-testing split and 5-fold cross-validation.  

I used three models to try to classify a statement as coming from a Republican or Democratic senator: Random Forest, Support Vector Machines, and Multinomial Naïve Bayes. I chose these three particular models because I had seen their use in a classification task that attempted to sort news articles into sections; I drew from this particular example because the vocabulary used in news articles and the direct nature of their content may provide similarities with the questioning style of congressional hearings. (They largely refer to specific events and are intended to be straightforward). I used ```sklearn``` to implement these models and Zafra's code (link) as a guide, and, behold:    

| Model                   | Training Set Accuracy | Test Set Accuracy |
|-------------------------|-----------------------|-------------------|
| Random Forest           | 0.857143              | 0.633333          |
| SVM                     | 0.987395              | 0.683333          |
| Multinomial Naïve Bayes | 0.894958              | 0.583333          |  

Overall, none of the three models performed to a standard that would allow for functionally correct classification of senators as either Republicans or Democrats based on their questions. It is important to note that since there were more Republican senators at the questioning than Democrats, there are more statements from Republican senators than Democratic ones, comprising 56.4 percent of all statements. Therefore, guessing that every statement belongs to a Republican would mean 56 percent accuracy. All three models are improvements on this baseline, but the best performance is from the Support Vector Machines (SVM) model, which yielded an accuracy of 98.7 percent on the training set and 68.3 percent on the test set. The worst performing model was the Multinomial Naïve Bayes (MNB) model, yielding 58.3 percent accuracy on the test set. Of all three models, it seems that the MNB model was the one that most over-fitted, and that the Random Forest least-overfitted.   

From a closer look at the confusion matrix of all of these models, we can see that the misclassifications that brought down the accuracy score were almost always false classifications of Democrats as Republicans. The poorest-performing model, the MNB, misclassified 23 out of 24 of the testing Democratic statements as Republican. SVM, despite having the best performance, still misclassified 14 out of 24 Democratic statements as Republican. It is interesting that the single Independent was most often misclassified as a Republican; the Independent in question, senator Angus King of Maine, often votes with Democrats. From this tendency throughout all three models, it is possible that the unbalanced sample (particularly prominent given the small overall sample size) is reducing the accuracy of results. 

*SVM confusion matrix:*  
<img src="images/svm.png?raw=true"/>  

### 4. Conclusions

Overall findings
