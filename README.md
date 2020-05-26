ADRMine is an NER (Named Entity Recognition) tool which is trained for extraction of medical concepts, particularly ADRs (Adverse Drug Reactions) from user posts in social media.

Requirements And Steps To run ADRMineDB

ADRMine uses MYSQL database, please install MYSQL if you don't have on your machine.

1) First create a mysql database, e.g. ADRMineDB:
           CREATE DATABASE `ADRMineDB`;

2) Restore the provided schema.

**How to use ADRMine?**
We use bazel (https://www.bazel.build/) To build an executable jar file for the project. To build the project using the command line, change your current directory to java_src folder e.g:

cd release/v1/java_src/

and build the jar file using the following command:
bazel build ADRMine:adrmine_deploy.jar

A jar file should be built in bazel-bin directory.

The jar file can be executed using the following command:
java -jar bazel-bin/ADRMine/adrmine_deploy.jar <input OPTIONS>

The current version of ADRMine includes the trained models for  ADR extraction. "MainADRMine is the main class related to training and testing ADRMine that expects different options as input arguments, below we listed the options for training and testing:
+ train : if the -train option exists, the program is in training mode, and if the option does not exists it is in testing mode.
+ dbName: is the name of the local schema that ADRMine connects to.
+ dbUser: the database username
+ dbPass: the database password
+ docs: The path to the train/test corpus. The text file contains train or test text contents. Every line is expected to have two elements that are tab separated, a unique ID associated with the post and the post text content. 
 e.g.: vyvanse-1234	Paxil completely gets rid of my appetite. not quite sure how to feel about this.

+ goldAnns (optional for testing): Is the gold standard annotations. Passing this file as an input argument is optional for testing. If the option exists, the system compares the IOB labels with the gold standard and prints performance results (precision, recall and F-score).
 For the posts that contain any relevant entity, there is one or more associated lines in the annotation file. For the posts that the human annotators did not tag any entity, we will not have a corresponding line in the annotation file.
 
+ corpusName: is the name that can be defined for the experiment. Please choose a different name for each separate experiment. Please note that the system is trained on user posts in health related social networks (e.g. DailyStrength.org) and  Twitter. We trained a different model for user tweets, so if your test sentences are from twitter, include “twitter” as part of your corpus name “e.g. twitter1”.
+ crfPath: is the path of the executable CRFSuite file in your system, after you install CRFSuite, get the path to the executable CRFSuite e.g.: /usr/local/Cellar/crfsuite/0.12/bin/crfsuite
+ modelFile: It is an optional parameter that is used during testing. If it is not passed in then the program uses the default, previously trained, model, otherwise it uses the model given as modelFile.
+ forTwitter: set to true if the experiment is on Twitter data and false otherwise. 
 
**Usage examples**

Example command to run the pertained ADRMine model on a test set of Tweets:
java -jar bazel-bin/ADRMine/adrmine_deploy.jar -dbName ADRMineDB -dbUser <your_username> -dbPass <your_db_pass> -docs test_sentences.tsv -corpusName twitter1 -crfPath /usr/local/Cellar/crfsuite/0.12/bin/crfsuite -forTwitter true

Example command to train ADRMine given the train sentences and gold annotations:
 -train -dbName ADRMineDB -dbUser user1 -dbPass your_db_password -docs train_tweets_id_content.tsv -corpusName twitter -goldAnns train_tweet_annotations.tsv -crfPath /usr/local/Cellar/crfsuite/0.12/bin/crfsuite -forTwitter true

Output:
The output file includes the text ID, start and end character offset, type, extracted text span

Example output line:

ID123	18	37	ADR	gain a lot of weight


Citing ADRMine
Nikfarjam A, et al. Journal of the American Medical Informatics Association 2015;0:1–11. doi:10.1093/jamia/ocu041






