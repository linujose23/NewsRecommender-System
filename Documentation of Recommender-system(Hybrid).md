                                            NewsRecommender-System(Hybrid)




* first we imported all the required Functions like Numpy(For numerical Operations),sklearn Cosine,NLTK Functions,Counter function to get unique words only we used it here we pick the keys.
,Pandas For Reading Data (News.csv)
For Vectorizing our Articles after removing all the un-necessary elements like Regular Expression and Tokenizing our Articles For (Vectorizing and to lighten our matrix)
the Cleaning of the Articles is done through Clean_Tokenize Function that has been created for cleaning the data and NLP functions have also been used to further process the data.

* Then we created an empty list  Total_words then we applied a For loop for Tokenizing each word in our cleaned_articles and Appended it to our empty List Total_words to further use it inside the counter Function.

* We used Counter function here only to get Unique words which are our(keys) and stored it in a variable VOcab we store all the keys from counts (Counter gives counts of words but we only need unique words in the form of dictionary).

* After getting unique words from the Counter Function we Remove Stop Words from it.

* Then we enumerate the Stops_removed(words after removing stop words) and define it as a dictionary to give our self Vocabulary in Tfidf Vectorizer else it will use predefined dictionary of Tfidf which contains lots of Unnecssary words Vocab so to avoid that we give our self VOcabulary thus it also gives a lighter matrix after Vectorizing.

* We Vectorize it after Giving our self Vocabulary (Final_vocab) in Tfidf Vectorizor thus we get the Vectors in the form of Matrix shape(4831 docs and some length of words in columns). 

* Now we used LatentDirichletAllocation(Topic-Modelling) in order to get Articles and Topics(which are Pre Trained in Neural Networks thus we only need to use it) here i have given 10 topics so our matrix we get will be (4831,10) 4831 docs and 10 Topics.

* Now we split it word by word to Calculate the average time to be spent by user to read the Article as mentioned down the Formula used here below.

                  
                   # Formula used to Calculate avg time,interest time Value and User_Profile :
                   
                   
                   
          * First we need to calculate the average time for all article,

                  as time = distance / speed

                  so in our case

                  time = length of article / time  (assumption  (5 word/sec))



                  for example:- user read article number 2 and 7, so we will pick the article vector of 2 and 7

                  2nd Article:- [0.1, 0.5, 0.3, 0.1]

                  7th Article:- [0.2, 0.3, 0.1, 0.4]

                  and time spend

                  2nd Article:- 300 sec

                  7th Article:-700 sec 



                  now time value

                  2nd article time / average time we calculated  (300/600)

                  7th article time / average time we calculated  (700/1000)



                  finally 

                  2nd article vector * 2nd article time value + 7th article vector * 7th article time value

                  ([0.1, 0.5, 0.3, 0.1] * (300/600)) + ( [0.2, 0.3, 0.1, 0.4] * (700/1000))

                  that is our user profile 

                  if we choose the vector as LDA(topic modelling) so we can normalize the final matrix that is our user profile



                  but there are some issues in it like

                  if user spend more time then our average time 

                  for that we use sigmoid 



                  for simplifying the calculations or speed up the process:-



                  make a vector of

                  user read article i.e [[0.1, 0.5, 0.3, 0.1], [0.2, 0.3, 0.1, 0.4]] name it user read article matrix



                  time value i.e [(300/600), (700/1000)] name it time value matrix

                  take sigmod of time value matrix



                         now

                        user read article matrix * sigmoid(time value matrix)

                        this is User profile

                        in case of LDA(Topic modelling use normalization)

 
* The above mentioned Formula is used to Create the User_Profiler Function from which we will get our User profiles 
  calculated so in this Function im passing three user read Articles and Wordtokenized Article 
  (To Calculate Avg time and here i have given words per second assumed by myself we take 5 words per second),
  and we give our an assumed time ourselves that is article_time(the time user actually spent on the particular article).
  
 #user_profile_generation :
 
 * from the User_profiler Function we get user profiles which in this case i have passed three Parameters of one users 
   so we get one user_profile based 
   on the interest of user from 10 topics from 3 articles read from
   topic_articles by user topic weights of all three articles summed thus we get a user profile.
 
 
 #content approach implemented :
 
 * now a content recommends function has been created which gives us cosine similarity to get top similar articles
   thus we picked top 10 similar articles by sorting it and reversing it to get top 5 scorers through sort which
   will return the top 10 similar scores which are similar to our user_profile and we append to all to a empty list of
   user_intrested_articles and then we implemented a 0.4 variation to our scores for recommending 40% Content based articles.
  
  #Collaborative approach implemented :
  
 * now we take cosine similarity of existing and new users and then we reverse it(ascending to descending order) in order to get                          our top scores through argsort it returns us the indexes of top 10 scorers now we pick our top 10 users from our existing users and then we mean it in order to get our common interest or collaborative interest of users now in order to recommend 30% of our collaborative user articles and then we implemented a 0.3 variation to our scores for recommending 30% Collaborative Filtering based.
 
 #Trending Approach implemented :

* so now we in addition we would also like to Recommend Trending Articles that are mostly been read by most so we take mean of    exisiting user profile and then we take cosine similarity of common interest of the existing users and LDA matrix thus we get more Trending similar articles to recommend and we pick top 10 Trending articles.


#Hybrid apporach :

now we add all the interests Content+collaborative+Trending scores to get a Hybrid score then we take cosine similarity of the the Hybrid Interest we get and the LDA matrix in order to get our Hybrid Recommendation scores now we use argsort and reverse the same to get the indexes of our similar Hybrid articles that are a mixture of content,collaborative and Trending articles and finally we recommend the articles to the users.
  
 
 
 
 #Recommending Articles based on Hybrid Apporoach :

   * We thus Finally  Recommend the top 20 Titles of articles.   
   **********************************************************************************************************************************

