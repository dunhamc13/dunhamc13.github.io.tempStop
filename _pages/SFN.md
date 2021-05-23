---
permalink: /SFN/
title: "Signal From Noise!"
---
 <style> .indented { padding-left: 10pt; padding-right: 10pt; } </style>
<style> .half {     display: block;
  margin-left: auto;
  margin-right: auto; width: 50%; } </style>
<style>
    figure {
    display: inline-block;
    margin: 20px; /* adjust as needed */}
    figure img {
      vertical-align: top;}
    figure figcaption {
    text-align: center;}
 </style>
## How to use social media to detect the signal from noise!  Predicting Violent Uprisings with Machine Learning
<center><img src ="https://github.com/dunhamc13/dunhamc13.github.io/blob/master/teaser.png?raw=true" class="half"></center>  
<p class="indented">This project, Signal From Noise (SFN), explores Special Forces Doctrine and social media as features to predict the use of violence to overthrow a government. SFN aims to produce a higher level of accuracy in contrast to traditional approaches that have focused their features on social and economic sciences.</p>
<p></p>
<center><img src ="https://github.com/dunhamc13/dunhamc13.github.io/blob/master/dayObj.png?raw=true" class="half"></center>  
<p class="indented">SFN is a machine of machines. The machine has a ’day’ object. Each day object has seven features. Those features are machines in their own right. The seven features are: hate speech, disinformation, disenfranchisement, Law Enforcement / Military Infiltration (LE/MIL Inf), organizing, political sentiment, and political attacks. The values of those features will be used to conduct the final model to predict a UW event.  Thus far, in the initial Design only the hate feature has been implemented.  </p>
<p></p>
<p class="indented">To reverse engineer our features we use the Special Forces Doctrine to monitor activities that lead to overt acts to overthrow a government. We begin at the bottom of the figure below. The base of the pyramid begins with measures of dissatisfaction. That dissatisfaction is aimed at political, economic, and social conditions. This base could be read as Political Sentiment and scraped from social media. As we move up the base we see elements of Disinformation with propaganda and lies. Again, this can be measured through social media data scrapes. To expedite this iterative process, elements of Disenfranchisement, LE/MIL Inf, Organizing, and Political Attacks all increase as we move up the figure towards the overt Unconventional Warfare (UW) thresh hold represented with a dashed line.</p>
<p></p>
<p><center><figure><img class="half" src ="https://github.com/dunhamc13/dunhamc13.github.io/blob/master/uw.png?raw=true"><img hspace="20"><figcaption>Special Forces Doctrine used to derive features</figcaption></figure></center></p> 
<p></p>
<p class="indented">One of the neat features of the program was creating a feature that presents a word blob of the words that are hate.  This feature was implemented to visually identify issues with processing or bias in the machine.</p>
<p></p>
<p><center><figure><img src ="https://github.com/dunhamc13/dunhamc13.github.io/blob/master/hate.png?raw=true" class="half" ><img hspace="20"><figcaption>Hate Word Blob</figcaption></figure></center></p>  
<p></p>
<p class="indented">The Hate Speech feature uses a Kaggle Twitter Sentiment Analysis data set consisting of 32,000 tweets. The tweets are contained in a csv file and have three columns. The first column is an id, the second is the label (hate, not hate), and the third is the tweets text. We will use these columns to create our features. Please note, a second data set without labels was provided but not used.</p>
<p class="indented">To extract our features we will examine the text through Natural Language Processing (NLP). NLP is a subfield of linguistics, computer science, and artificial intelligence that examines the inter- actions between computers and human languages. One of the main challenges presented in NLP is natural language understanding (NTU). NTU deals with machine reading comprehension. This was one challenge that our current implementation needs to continue to focus on delivering better metrics.</p>
<p class="indented">Dealing with language provides many different options for data transformation. It was necessary to build a new data structure that would be examined for patterns with our models. I took our original data structure and began adding additional columns. First, I parsed each tweet and made a words per tweet column. The thought process here was that when people are angry, they might write longer tweets.</p>
<p class="indented">Next, under the premise that hate words could vary in length from not-hate words I made another feature chars per tweet. Go- ing along the same line of thought about length my next column extracted the average word length per tweet.</p>
<p class="indented">As alluded to in the previous section, the initial KNN model with 7 features per- formed rather poorly. See Figure 2: 7 Fea- tures KNN demon- strating an accuracy that was consistently above 90%. However,the f1-score remained in a range between 7 and 15. This led to much experimentation. My initial thought was that I lacked enough fea- tures to get a good result. However, while I examined different options the quickest modification I could make would be to implement other classifiers.</p>
<p></p>
<p><center><figure><img class="half" src ="https://github.com/dunhamc13/dunhamc13.github.io/blob/master/CM_RF.png?raw=true"><img hspace="20"><figcaption>Confusion Matrix Random Forrest</figcaption></figure></center></p>  
<p></p>
<p class="indented">After implementing a decision tree and SVM classifier I was able to achieve the following metrics. KNN was the quickest at 5,038,503 nanoseconds and an average accuracy of 92%, but a hate tweet f1-score of 0.15. The decision tree was second quickest at 9034925 nanoseconds and an average accuracy of 92%. These results also rendered a hate tweet f1-score of 0.09. The SVM model was not able to identify any hate tweets, and was trained in 6,045,222,865 nanoseconds. Most interesting was that it still maintained an aver- age accuracy of 92%.</p>
<p class="indented">That result speaks volumes as to why I do not believe that ac- curacy is a relevant metric for hate speech. SVM did not register a single hate tweet, but had the same accuracy as the other models. This shows the unbalanced nature of the classes.</p>
<p class="indented">From this point I began implementing the TF-IDF structure and was able to create the TF-IDF structure to test it prior to designing a method to merge the previously extracted data features with new TF-IDF features. The TF-IDF feature provided the model with 38,441 uniquely extracted features. At this time I also included a 4th model, random forrest (RFM). I chose to try this method as the ’decision tree’ of ’decision tree’s’ idea appealed to me on the grounds that with so many features it might be better at finding a more accurate f1-score han my manual implementation of different decision trees.</p>
<p></p>
<p><center><figure><img class="half" src ="https://github.com/dunhamc13/dunhamc13.github.io/blob/master/ROC_RF.png?raw=true"><img hspace="20"><figcaption>ROC Curve</figcaption></figure></center></p>  
<p></p>
<p class="indented">This combination produced the most satisfactory results of my project. The RFM trained in 10,712,679,091 nano seconds producing a 96% accuracy with a best run including an f1-score that was clocked at 0.64 for hate tweets. All other models saw significant improvement as well. KNN trained in 2,171,628 nano seconds, increasing accuracy by 1% to 93% and nearly doubling the hate tweet f1-score to 0.23. The decision tree trained in 655,696,436 nano seconds with a 94% accuracy and a 0.38 hate tweet f1-score. Please refer to Appendix E : Decision Tree to view the max depth of 6 tree that was produced. Finally, even the SVM model showed tremendous improvement. SVM continues to take the longest to train at 119,283,695,507 nano seconds, but jumped to a 94% accuracy with a second best hate tweet f1-score of 0.40.</p>
<p class="indented">Future work will focus on delivering a higher F1 score for the hate speech model before implementing the other 6 features.</p>



