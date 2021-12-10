# Gutenberg-Gensim-word2vec-evaluation-project

This project aims to evaluate the genre classification of the 69 most popular eBooks in the Gutenberg project collection (https://www.gutenberg.org/browse/scores/top). Each book in this collection has multiple genres included in its metadata, and this analysis will dive into evaluating the consistency of these genres on a high level. 

Given the high specificity of genres, we will only study whether the highest-level categories available, which are Fiction (At least one Subject contains Fiction as a direct category or hierarchical category), vs Non-Fiction (None of all Subject contains Fiction as a direct category or hierarchical category). We will analyze if books classified as Fiction are similar to all other fiction books in this collection from a bag-of-words perspective. In other words, we will see if books classified as Fiction, share a similar vocabulary to all other books under that same category. 

We were able to scrape and categorize the following number of books:

#Fiction Books:  43
#Non-Fiction Books:  26

We created a Fiction and Non-Fiction vector representation by aggregating all these 69 documents into vectors that represent each of these two categories (Fiction vs Non-Fiction). After that, we compared new documents (embedded with the same model) to these vectors by using cosine similarity to check if they were classified as Fiction or Non-Fiction in a consistent way. If a document was classified as some kind of Fiction and is more similar to the Fiction vector than to the Non-Fiction vector, then that would mean that that specific book was categorized consistently from a vocabulary perspective. In other words, this book has similar vocabulary as the books under the same high-level category.

Technical Specification

The analysis process consists of the following steps:
1.	Scrape 69 of the 100 most popular books in Gutenberg project. 
2.	Manually assign the most relevant genres associated for each book. These are stated as “Subject” in the e-book page for each book (Eg: See https://www.gutenberg.org/ebooks/84 > Bibliographic Record > Subjects). Because we are only analyzing a low number of books, we only assigned low-specificity genres and not location-related were assigned in our analysis.
3.	We re-classified each book into Fiction or Non-Fiction categories, based on if any genre of that book contained the word “fiction” or not.
4.	We create a Gensim doc2vec model to build genres and document embeddings. Gensim doc2vec library creates a bag-of-words (dense vector) representation of each document, that accounts for the order and semantics of words in the document. 
5.	We train a model using three types of labels: Fiction, Non-Fiction, or other. The latter are those documents which will be used to evaluate our model (hence, these book’s classifications into one of the two categories). With this model, we are able to obtain a vector representation for Fiction books, Non-fiction books, and individual books (which will be used for the evaluation).
6.	Finally, we calculate cosine similarity between each TEST book against Fiction and Non-Fiction vectors, to see where it should be classified from a bag-of-word perspective, vs how it was actually classified.

More details are included in the python code section.


RESULTS:

We can see most of fiction categories were assigned properly. 

Only 13 of a total of 38 books were not classified correctly, yielding a 65.8% success rate.

The expections were the following:
- Jane Eyre: An Autobiography by Charlotte Brontë
- Don Quixote by Miguel de Cervantes Saavedra
- A Doll's House : a play by Henrik Ibsen
- Kwaidan: Stories and Studies of Strange Things by Lafcadio Hearn
- Frankenstein; Or, The Modern Prometheus by Mary Wollstonecraft Shelley
- Les Misérables by Victor Hugo
- War and Peace by graf Leo Tolstoy
- Dracula by Bram Stoker
- A Tale of Two Cities by Charles Dickens
- The American Diary of a Japanese Girl by Yoné Noguchi
- Ulysses by James Joyce
- Wuthering Heights by Emily Brontë
- The Romance of Lust: A classic Victorian erotic novel by Anonymous

This is a simple example of categories evaluation by using Ginsem doc2vec embeddings. Categories with more specificity can be studied with this same model but a much larger data set must be extracted to obtain representative genre vectors that can be used for evaluating books genres.


References:

Document embeddings and Gensim doc2vec models
-	https://radimrehurek.com/gensim/models/doc2vec.html#module-gensim.models.doc2vec
-	https://arxiv.org/pdf/1405.4053v2.pdf 
-	https://towardsdatascience.com/multi-class-text-classification-with-doc2vec-logistic-regression-9da9947b43f4
-	https://www.kaggle.com/currie32/comparing-books-with-word2vec-and-doc2vec/notebook#Doc2Vec

Scrapping Gutenberg project books
-	https://katherinepully.com/project-gutenberg-scraper/
-	https://codereview.stackexchange.com/questions/141898/sample-scraping-project-gutenberg-using-beautiful-soup-and-requests
