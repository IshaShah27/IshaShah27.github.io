## Jim Comey, Senate Intelligence Committee, June 2017: A test site for NLP techniques
*What can Jim Comey's testimony tell us about using natural language processing techniques to analyze court proceedings?*

Contextualization is more difficult for machines than humans. As current ML techniques stand, this is especially true in tasks related to understanding natural language - language as it is freely spoken or written by individuals, rather than one that has been designed, like a computer language. Full of idioms and turns of phrase, individual eccentricities, and complicated tones, it can prove difficult for any of us, machine or human, to understand what others are saying.  
  
The halls of Congress, for example, are often home to proceedings that flummox both human and machine interpreters. While humans are able to easily understand what the members of the house or senate committees and the respondents are saying, it can be difficult to assess what they *mean*. Computers, with a perhaps more rudimentary grasp of tone and expression, face an even greater challenge. Throw in the soup of motivations of all parties bubbles in the background of these questionings and the verbal contortions that respondents often make, and it seems truly impossible that, on the surface, natural language processing techniques can tell us anything about these proceedings that the hundreds of politicos, journalists, and talking heads across the country who are paid to perform the very task of interpreting the testimony cannot. And yet, applying these techniques in this context is *extremely* tempting:  

  - For one, **most congressional testimonies are important and influential**. Even if there is a marginal improvement to be had in our understanding of these proceedings, it could have large a large impact. 
- Transcripts are legally required to be made and released to the public after an open testimony, providing a **clean, high-quality, and easily processed** dataset that is almost unparalleled in its ease of access.
- Focusing on only text as data, rather than visual cues, might allow for analysts to understand testimonies in a way that is **free of visual bias** stemming from one’s appearance and inferred demographics, whether related to age, race, gender, or any other visual presentation.
- NLP techniques allow for the simultaneous **processing of large volumes of testimonies at once**, allowing analysis of a corpus of data whose size would be prohibitive for qualitative research.
- NLP techniques also present a **unified framework** that can be applied across different testimonies. These frameworks could be used to compare performances by senators or representatives or testifiers across hearings, or compare how hearings on the same subject are distinctive depending on who the senators or representatives are.

To me, it seems like applying NLP techniques to transcripts of court proceedings could open up a whole new realm of analytical possibility. And yet, it is clear that they might have to be applied awkwardly, and that there may be some trial-and-error in finding the questions these techniques can and cannot be used to answer. So, I went for it.  
  
  In order to explore some of the potential uses and pitfalls of applying NLP techniques to congressional testimonies, I used former FBI Chief James Comey's testimony before the Senate Intelligence Committee on June 8th, 2017. This testimony in particular provides a particularly rich corpus on which to test NLP techniques, because James Comey's political position in June 2017 was fascinatingly complicated. The purpose of the Congressional hearing was to learn about the circumstances of Comey's firing from his role as FBI Chief, which Comey suggested was due to his failure to comply with President Trump's requests for his "loyalty" in matters related to the Russia investigation, rather than his job performance.
    
**The current hypotheses:**  
- Republican senators may treat Comey more favorably than their Democratic counterparts. Comey has been vocal in confirming that the president is not under investigation and has also served under other Republican presidents as attorney general, making him perhaps a sympathetic figure within the Republican party. Democrats may be more willing to take a more aggressive tack toward his questioning - many may believe that it was Comey’s announcement of the re-opening of an investigation against Hillary Clinton, the Democratic presidential candidate, that led to the party’s loss in the 2016 election. *(Method: sentiment analysis) *
- The possibility that there is a distinction in the topic and tone of questioning between senators of different parties also introduces the opportunity to do a supervised learning exercise in order to identify the party of the questioner. If a well-tuned interpretable machine learning model can predict the party of a questioner above the level of chance, it could reveal distinctions between parties and senators that are not apparent from qualitative analysis. *(Method: supervised classification)*
**Questions for future analysis:**  
- Which topics do Republican and Democratic senators focus on? The answers yielded by an NLP analysis would be useful to compare to those of human readers, to determine if topic modeling techniques can parse this sort of testimony well enough to be applied to large numbers of testimonies at once. While human interpretations may be more accurate for a single testimony, research that requires reading through volumes, perhaps for a historical analysis, could be helped tremendously by machine learning techniques - if they perform well enough.
- Each of the senators on the Select Intelligence Committee has taken part in many other hearings, as has Jim Comey. How do their performances compare across these different contexts?
- 

### 1. Word clouds

Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. 

<img src="images/dummy_thumbnail.jpg?raw=true"/>

### 2. Assess assumptions on which statistical inference will be based

```javascript
if (isAwesome){
  return true
}
```

### 3. Support the selection of appropriate statistical tools and techniques

<img src="images/dummy_thumbnail.jpg?raw=true"/>

### 4. Provide a basis for further data collection through surveys or experiments

Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. 

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).
