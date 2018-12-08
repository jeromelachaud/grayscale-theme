# What is the Book Network?
Welcome to Book Network, the network analysis project of the 3000 most popular book from GoodReads.
We love literature and we are studying *Social graphs and interactions* at Technical University of Denmark so this is the pinnacle of our efforts.

## How do we make the network?
The cornerstone of our analysis are the 3000 most rated books with the highest average rating on GoodReads. It is the largest website where people rate and review books. 

We took these books and created a connection whenever the same person rates both of the books. As the result we've got a graph where books are the nodes and edges carry the number of people who rated both of the books.
This way, we've created network of **Fans** and network of **Haters**. 

Fans are the users who liked the books and rated them 4 or 5 stars. Haters disliked the books and gave them rating 3 or 4 stars. 

## What we want to learn?
- Why people like the popular books? 
- What are the communities within the network - what are the groups of books read by the same people?
- What are the unpopular opinions about such well recieved books? 
- Why there is still a considerable number of people who dislike them?

## Recommendations
And finally we will create couple of recommendations based on the communities we discovered. We will find the most central books and recomend you some interesting titles which you shouldn't miss out. But that's not all. We will also pick most controversial books and advise what titiles to avoid, if you don't like them.

# The Network
//todo

# The data we used
We used a subset of Goodbooks-10k from Github, which contains most rated books, their user ratings and all the tags users give them. However we wanted even more information so we can delve into text analysis. Therefore we wrote a python script to download and extract book descriptions and reviews directly from Goodreads.
In total it is around X MB of data. You can download [Goodbooks-10k](https://github.com/zygmuntz/goodbooks-10k) and [CSV]() with book descriptions and reviews to try to play with the data. If you want to see more details about how we processed and analysed our data, you can find those in the [Explainer notebook](). 

# Analysis
Now you know everything about our idea and how we created the networks of Fans and Haters. Let's look at how we analysed them.
The main idea behind the analysis of Book Network is finding communities of books. We want to find out what groups of books people read and try to answer - why? At first we expected people tend to stick to couple of genres when they like, so the communities would be prevalently genre based. But as we progressed with our analysis, we found out it is not exactly true - the tastes in literature are much more varied. But before we delve into what we've found out, let's look at how we did it.

## Finding communities
At first we had to identify communities within our networks. We used the Louvain Community detection algorithm for that. 
This algorithm optimizes modudularity, which is a measure of density of connections within community compared to connections to other communities. The optimization is performed in two steps. Firstly, the method looks for "small" communities by optimizing modularity locally. Second, it aggregates nodes belonging to the same community and builds a new network whose nodes are the communities. This is repeated until maximum modularity is achieved. On the downside, it is harder to detect small communities within large network, because they are often merged together into a bigger community. It might explain why the book communites often contain groups of books on several topics. 

Resulting communities often contain more than 100 books. In the further analysis we've used top 50 most connected books within each community, to get more easily interpretable results. These books were read by most of the users, therefore we believe this sample is good representation of the community.
[list of communities]

## Tags
### *"What are these books?"*
To find out what kinds of books are within each community we looked into tags. These are created by the users, when they categorise the books in their GoodReads library. We took 5 most frequent tags for each book. However we've excluded several popular tags, which don't tell us much about the content of the book, rather how does the user got the book: #audiobook #library #owned-books
Then we pooled together the top 5 tags of the most connected books in the community and displayed the most frequent ones in word clouds.
[wordcloud example]


## Reviews
### *"What do people like about these books?"*
We also wanted to find another aspect what ties the communities together - how people feel about the books in the community. In case of Fan Network what are the things that people liked about the books. And as for Haters, what they disliked. 
We tried to answer that using sentiment analysis of book reviews. We took reviews of the top 50 most connected books. But we've found out there is quite a lot of them written in different languages. So we've decided to consider only reviews in English, we used a neat library for detecting languages *langdetect* to do that. Why? Simply because we want to understand our word clouds, since we are all about ~~aeromancy~~ cloud interpretation. However we've got some nice stats about the language distributions within communities.
We proceeded to calculate mean sentiment and standart deviation to choose only the most positive and most negative reviews for the community. Then we compute TF-IDF for these reviews to find the most important words. The algorithm allows us to find frequent words, which are also uniqe for each community. And finally we make our favorite word clouds.
[example word cloud]


# Fan Network
//todo


# Haters Network
//todo

# Fans vs Haters Comparison
//todo

# Recommendations
//todo

References
- https://neo4j.com/docs/graph-algorithms/current/algorithms/louvain/
- https://en.wikipedia.org/wiki/Louvain_Modularity
- https://perso.uclouvain.be/vincent.blondel/research/louvain.html
- https://pypi.org/project/langdetect/
- https://github.com/zygmuntz/goodbooks-10k

