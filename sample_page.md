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
  
  In order to explore some of the potential uses and pitfalls of applying NLP techniques to congressional testimonies, I used former FBI Chief James Comey's testimony before the Senate Intelligence Committee on June 8, 2017. This testimony in particular provides a particularly rich corpus on which to test NLP techniques, because James Comey's political position in June 2017 was fascinatingly complicated. The purpose of the Congressional hearing was to learn about the circumstances of Comey's firing from his role as FBI Chief, which Comey suggested was due to his failure to comply with President Trump's requests for his "loyalty" in matters related to the Russia investigation, rather than his job performance.
    
**The current hypotheses:**  
- Republican senators may treat Comey more favorably than their Democratic counterparts. Comey has been vocal in confirming that the president is not under investigation and has also served under other Republican presidents as attorney general, making him perhaps a sympathetic figure within the Republican party. Democrats may be more willing to take a more aggressive tack toward his questioning - many may believe that it was Comey’s announcement of the re-opening of an investigation against Hillary Clinton, the Democratic presidential candidate, that led to the party’s loss in the 2016 election. *(Method: sentiment analysis)*
- The possibility that there is a distinction in the topic and tone of questioning between senators of different parties also introduces the opportunity to do a supervised learning exercise in order to identify the party of the questioner. If a well-tuned interpretable machine learning model can predict the party of a questioner above the level of chance, it could reveal distinctions between parties and senators that are not apparent from qualitative analysis. *(Method: supervised classification)*  
  
**Questions for future analysis:**  
- Which topics do Republican and Democratic senators focus on? The answers yielded by an NLP analysis would be useful to compare to those of human readers, to determine if topic modeling techniques can parse this sort of testimony well enough to be applied to large numbers of testimonies at once. While human interpretations may be more accurate for a single testimony, research that requires reading through volumes, perhaps for a historical analysis, could be helped tremendously by machine learning techniques - if they perform well enough.
- Each of the senators on the Select Intelligence Committee has taken part in many other hearings, as has Jim Comey. How do their performances compare across these different contexts?

### 0. Pre-processing  
I removed a standard set of stopwords from the ```NLTK``` package, plus a few context-specific phrases that don't convey additional information: "ever said", "attorney general", and "Mr Comey" among them. I also lemmatized the text (reduced words down to their root, so that "ran" and "running" would be recorded as the same word, as would "was" and "been").  

**Some notes on pre-processing:**  
- *Question-answer relationship*: In this analysis, I considered each question and response independently. However, sentiment and topic analyses that take the relationship into account could be useful. For example, does the sentiment of the question-answerer match that of the question-asker? Does one vary while the other remains constant? Are questions on certain topics more or less inclined to evoke sentiment-laden responses from the answerer?
- *Quote gremlins*: Often, when a senator asks a question of Comey, she or he would quote a part of his written record of his meeting with President Trump or the president's tweets. That a senator chooses to quote a certain conversation or document, or even chooses to quote at all, is valuable information - but more careful pre-processing is required to remove these instances where they interfere with the analysis (i.e., sentiment analysis), and include them where they are additive (topic modeling, perhaps).
- *Introductory remarks*: For the purpose of this analysis, which was to determine the sentiment and style of the questioning portion of the proceedings, I did not include introductory remarks. These remarks are highly illustrative - senators often state directly their relationship with the person on the stand, and their opinion about the proceedings as a whole. Can these remarks perhaps serve as a check to an analysis of the questions, or provide useful context? Is excluding them willfully ignoring important background knowledge, or does an analysis of questions alone useful precisely because it lacks this information?

### 1. Word clouds

Without further ado, word clouds:

*Republican senators*  
<img src="images/rwc.png?raw=true"/>
  
*Democratic senators*  
<img src="images/dwc.png?raw=true"/>
  
*Comey*  
<img src="images/comey.png?raw=true"/>

**Observations:**  
- While senators of both parties feature the word investigation quite prominently, there seems to be a much greater diversity of words that appear with some frequency in the Republican word cloud than the one for Democrats. This could mean that when Democratic senators questioned Comey, they tended to make specific references to a limited set of discrete topics: “Flynn” (the investigation of Michael Flynn, a former Trump campaign aide), “dinner”  and “meeting” (the dinner Trump and Comey had together in which Comey claims the president asked for his loyalty, and “session” and “recusal” (in reference to Trump’s dissatisfaction with the recusal of Jeff Sessions, attorney general, from the Russia investigation).
- In contrast, the smaller size and greater number of the words in the Republican word cloud suggest that Republicans were perhaps more general in their questioning style and broader in the topics they covered – the same terms that are prominent in the Democratic word cloud highlighted above are much less so in the Republican word cloud, and instead words like “thing,” “conversation” (rather than “meeting” or “dinner”) appear. There are also fewer proper nouns in the Republican word cloud than the Democratic one, suggesting that their questioning may not have been quite as specific.
- In clear contrast to either of the senators’ word clouds, Comey’s contains considerably more words that suggest uncertainty, and they are featured prominently: “don’t know”, “something”, “well”, “thing”, “hope”, “might”, “don’t remember". This is also likely the case because Comey is more likely to convey how he felt or remembered his event – in other words, personally experienced it, rather than reporting an account that is as clear as that of a third-party observer. 
- 	In future analyses of congressional testimonies, particularly those where Comey or the same senators are involved, it would be interesting to see whether the differences in word frequencies seen in this example persist. For example, are these Democratic senators always more likely to ask questions about specific people and events to a greater extent than the Republican senators, or would it change depending on the political nature of the testifier or the subject matter the hearing was on? It would also be interesting to see whether James Comey’s responses are similarly indicative of uncertainty and personal experience in other testimonies compared to this one, which was about events directly related to the circumstances of his firing.

### 2. Sentiment analysis  
  
Many senators of both political parties have personal or professional relationships to Comey, and those who do not are aware of his background, career, and role in the investigation of Hillary Clinton. These relationships and prior knowledge have the potential to influence the tone of questioning, and could offer information to evaluate the accuracy of sentiment analyses. I used four tools for analysis: 
1. AFINN sentiment lexicon, a list of 2,477 English terms that were manually rated as either positive or negative on an integer scale of -5 to +5;
2. VADER (Valence Aware Dictionary and sEntiment Reasoner), a both dictionary and rule-based sentiment analysis tool that gives an average positive, negative, and neutral score that sums to 1. VADER takes into account several complexities of language that AFINN, a simple dictionary-based approach, does not: it accounts for common negations, contractions, and degree-modifiers (“very” to convey an emotion more strongly, for example);
3. NRC emotion lexicon, a dictionary that contains 10,000 words classified to eight emotions: anger, fear, disgust, joy, surprise, anticipation, sadness, and trust, where a 0 indicates absence and 1 indicates presence; and
4. NRC VAD (Valence-Arousal-Dominance) lexicon, a dictionary that contains 20,000 words that are classified on a 0-1 continuous scale on valence, dominance, and arousal, which are three scales of meanings for words. Valence is on the positive-negative scale, dominance is on the dominant-submissive scale, and arousal is on the active-passive scale.  
All measures are averaged over statement length.

***AFINN:*** AFINN scores for almost every group are almost exactly in the middle of the scale, indicating a tone that is neither positive nor negative. Comey has the highest AFINN average score, 0.145, which is still rather neutral, but could suggest that his responses used more emotionally charged terms than questioners of either party. The score difference between Republicans and Democrats (0.041 higher for Republicans) seems negligible, especially considering that the average valence values are so small to begin with.

*AFINN sentiment score:*  

| Group      | AFINN average score |
|------------|---------------------|
| Comey      | 0.145               |
| Democrat   | 0.028               |
| Republican | 0.069               |
  
***VADER:*** From the VADER scores, we can see once again that there is not much variation in sentiment valence between Democratic and Republican senators, but that there is now a more significant difference between the senators and Comey, who has a higher average positive and negative score and lower average neutral score. Comey’s VADER neutral score, for example, is 0.708 compared to 0.893 for Democrats and 0.884 for Republicans; his negative score is 0.116 compared to 0.037 and 0.038 and his positive score is 0.176 compared to 0.069 and 0.078 for Democrats and Republicans respectively. These scores suggest Comey was speaking in more emotionally charged terms and support the implication of Comey’s word cloud, which suggested he was responding in a way that emphasized his personal viewpoint.
  
An analysis of VADER scores by sentiment also yielded some interesting results:  
- The senator with the highest VADER neutral score, Democratic senator Kamala Harris of California (0.951) is known for being an especially direct prosecutor. It may make sense that she uses less charged language.
- The senator with the second-lowest VADER neutral scores is Cornyn, a Republican senator from Texas (0.807). Cornyn is known to be a longtime critic of Hillary Clinton and a potential replacement for Comey for the role of FBI Chief, a position perhaps reflected in his slightly lower neutral and slightly higher positive score.
- However, the lowest neutral score from a senator is that of Democratic senator Diane Feinstein of California (0.779), who also has the highest positive score of any senator, and even exceeds Comey (0.187). On the surface, this may also seem in line with expectations despite party affiliation: she has worked with Comey before, and has mentioned that she respects him. The crux is that she repeats a few phrases that the Comey says the president said in their discussion, a request for “honesty” and “loyalty”. While it is possible that senator Feinstein’s low neutral score and high positive score is driven by the spirit of her questions and tone, it may also be these quoted words, both of which have a positive valence.
  
***NRC:*** The NRC Lexicons did not yield much additional information - scores on the Anger, Fear, and Digust scales were quite low, and the Valence, Arousal, and Dominance scores suggested that Comey's speech was less dominant than either of the senators (understandable, given his position as question-answerer), and lower in arousal and valence (a little less intuitive, given his more polar VADER scores).
  
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
| BURR      | Republican  | -0.029        | 0.079          | 0.884         | 0.036          | -0.081         | 0.428       | 0.35          | 0.444       | 0.039     | 0.078    | 0.014       |
| HEINRICH  | Democrat    | -0.058        | 0.063          | 0.894         | 0.042          | -0.018         | 0.449       | 0.341         | 0.419       | 0.038     | 0.053    | 0.026       |
| REED      | Democrat    | -0.026        | 0.092          | 0.818         | 0.09           | 0.013          | 0.418       | 0.335         | 0.429       | 0.04      | 0.065    | 0.034       |
| HARRIS    | Democrat    | 0.018         | 0.018          | 0.951         | 0.032          | 0.053          | 0.412       | 0.302         | 0.43        | 0.008     | 0.018    | 0.003       |
| BLUNT     | Republican  | 0.077         | 0.046          | 0.888         | 0.066          | 0.077          | 0.448       | 0.316         | 0.39        | 0.01      | 0.01     | 0.005       |
| COTTON    | Republican  | -0.01         | 0.025          | 0.918         | 0.057          | 0.097          | 0.448       | 0.334         | 0.427       | 0.031     | 0.037    | 0.02        |
| McCAIN    | Republican  | 0.006         | 0.031          | 0.916         | 0.054          | 0.105          | 0.419       | 0.32          | 0.416       | 0.038     | 0.05     | 0.015       |
| LANKFORD  | Republican  | 0.096         | 0.032          | 0.882         | 0.086          | 0.119          | 0.441       | 0.286         | 0.394       | 0.005     | 0.021    | 0.005       |
| MANCHIN   | Democrat    | 0.088         | 0.032          | 0.899         | 0.069          | 0.121          | 0.473       | 0.354         | 0.422       | 0.004     | 0.02     | 0.011       |
| COLLINS   | Republican  | 0.029         | 0.015          | 0.943         | 0.042          | 0.123          | 0.361       | 0.285         | 0.355       | 0.029     | 0.028    | 0.037       |
| COMEY     | Comey       | 0.145         | 0.116          | 0.708         | 0.176          | 0.128          | 0.397       | 0.256         | 0.377       | 0.011     | 0.027    | 0.01        |
| WYDEN     | Democrat    | -0.029        | 0.03           | 0.911         | 0.059          | 0.144          | 0.437       | 0.323         | 0.442       | 0.01      | 0.013    | 0.01        |
| KING      | Independent | 0.08          | 0.022          | 0.883         | 0.095          | 0.186          | 0.426       | 0.304         | 0.402       | 0.003     | 0.025    | 0           |
| RISCH     | Republican  | 0.288         | 0.038          | 0.822         | 0.14           | 0.239          | 0.512       | 0.294         | 0.444       | 0.016     | 0.036    | 0.01        |
| FEINSTEIN | Democrat    | 0.183         | 0.034          | 0.779         | 0.187          | 0.278          | 0.39        | 0.3           | 0.384       | 0.005     | 0.005    | 0           |
| RUBIO     | Republican  | 0.086         | 0.015          | 0.901         | 0.084          | 0.322          | 0.352       | 0.245         | 0.324       | 0.017     | 0.019    | 0.025       |
| CORNYN    | Republican  | 0.113         | 0.04           | 0.807         | 0.154          | 0.413          | 0.429       | 0.324         | 0.413       | 0.028     | 0.031    | 0.021       |
| WARNER    | Democrat    | 0.041         | 0.021          | 0.908         | 0.071          | 0.419          | 0.467       | 0.339         | 0.435       | 0         | 0.003    | 0           |


### 3. Classification

The use of a classification exercise on a single proceeding is of potentially limited use. However, understanding the reliability of classification in a single instance may suggest how readily it can be used across longitudinal analyses. Identifying the instances in which classification algorithms are successful and those in which they aren't could be useful to see whether the distinctiveness of these two parties is apparent across topics, and over time.

For this analysis, I vectorized the corpus using Term Frequency – Inverse Document Frequency, because the goal was to highlight differences between the two parties. I also used both unigrams and bigrams, since certain phrases like “Russia investigation” may make more sense together than apart. I did not stipulate a minimum or maximum document frequency, because the number of questioners and the number of statements were both relatively small, and the use of rare words or phrases by senators is relevant in this context.  

Since the classification was intended to answer the question of whether senators could be distinguished by their questions, I dropped Comey’s statements from the dataset, leaving 168 statements by Republicans, 110 statements by Democrats, and 20 statements by the Independent. I used an 80-20 training-testing split and 5-fold cross-validation.  

I used three models to try to identify whether a question was asked by a Republican or Democratic senator: Random Forest, Support Vector Machines, and Multinomial Naïve Bayes. I chose these three particular models because I had seen their use in a classification task that attempted to sort news articles into sections; I drew from this particular example because the vocabulary used in news articles and the direct nature of their content may provide similarities with the questioning style of congressional hearings. (They largely refer to specific events and are intended to be straightforward). I used ```sklearn``` to implement these models and Zafra's code (link) as a guide, and, behold:    

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



**TLDR;** An analysis of James Comey’s testimony before the U.S. Senate Intelligence Committee in June 2017 using natural language processing (NLP) techniques provides an example through which to explore the use of these techniques on court transcripts. 
- NLP techniques may be able to provide findings complementary to qualitative research - they can allow for large volumes to be analyzed simultaneously using a consistent framework and remove visual sources of bias.  
- A word frequency exploration of Comey’s testimony shows more proper noun use among Democratic senators, less specificity among Republican senators’ questions, and more uncertainty and personally-centered words in Comey’s replies. 
- Sentiment analysis using four different tools suggests that Comey’s statements are less neutral than senators’ questions from either party, and that the dominance of his statements is also lower. However, quoting of written testimony during questioning may be a confounding factor.  
- Across three classification machine learning models (Random Forest Classifier, Support Vector Machines, and Multinomial Naïve Bayes), the accuracy of classifying senators as Democrats or Republicans based on their questioning reaches at most 0.68, suggesting that the questioning style of senators is not perfectly differentiable by party, and that oversampling may have potentially large confounding effects.
