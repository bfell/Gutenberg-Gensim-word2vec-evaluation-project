# Gutenberg Genre Evaluation using Gensim word2vec

This project aims to evaluate the genre classification of the 69 most popular eBooks in the Gutenberg project collection (https://www.gutenberg.org/browse/scores/top). Each book in this collection has multiple genres included in its metadata, and this analysis will dive into evaluating the consistency of these genres on a high level. 

Given the high specificity of genres, we will only study whether the highest-level categories available, which are Fiction (At least one Subject contains Fiction as a direct category or hierarchical category), vs Non-Fiction (None of all Subject contains Fiction as a direct category or hierarchical category). We will analyze if books classified as Fiction are similar to all other fiction books in this collection from a bag-of-words perspective. In other words, we will see if books classified as Fiction, share a similar vocabulary to all other books under that same category. 

Books scrapped:

#Fiction Books:  43
#Non-Fiction Books:  26

I created a Fiction and Non-Fiction vector representation by aggregating all these 69 documents into vectors that represent each of these two categories (Fiction vs Non-Fiction). After that, we compared new documents (embedded with the same model) to these vectors by using cosine similarity to check if they were classified as Fiction or Non-Fiction in a consistent way. If a document was classified as some kind of Fiction and is more similar to the Fiction vector than to the Non-Fiction vector, then that would mean that that specific book was categorized consistently from a vocabulary perspective. In other words, this book has similar vocabulary as the books under the same high-level category.

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

Cosine Similarities with Fiction vs Non-Fiction Embeddings:

The Romance of Lust: A classic Victorian erotic novel by Anonymous (220)    ['Incest -- Fiction', 'Erotic stories']
Fiction similarity:  [[-0.05316773]]
Non-Fiction similarity:  [[0.02405052]]

A Pickle for the Knowing Ones by Timothy Dexter (486)    ['Biography']
Fiction similarity:  [[0.03112186]]
Non-Fiction similarity:  [[0.27475834]]

Märchen der Gebrüder Grimm 2 by Jacob Grimm and Wilhelm Grimm (220)    ['Fairy tales -- Germany']
Fiction similarity:  [[0.08288112]]
Non-Fiction similarity:  [[0.36124396]]

Anna Karenina by graf Leo Tolstoy (238)    ['Adultery -- Fiction', 'Didactic fiction', 'Love stories']
Fiction similarity:  [[0.39001006]]
Non-Fiction similarity:  [[0.22117396]]

Wuthering Heights by Emily Brontë (268)    ['Psychological fiction', 'Love stories', 'Domestic fiction']
Fiction similarity:  [[0.0976584]]
Non-Fiction similarity:  [[0.19259836]]

Ulysses by James Joyce (390)    ['Psychological fiction', 'Domestic fiction', 'Epic literature', 'Married people -- Fiction', 'Male friendship -- Fiction']
Fiction similarity:  [[0.03295119]]
Non-Fiction similarity:  [[0.08621626]]

Tractatus Logico-Philosophicus by Ludwig Wittgenstein (320)    ['Philosophy']
Fiction similarity:  [[0.07726576]]
Non-Fiction similarity:  [[0.35948765]]

The American Diary of a Japanese Girl by Yoné Noguchi (623)    ['Japan -- Fiction']
Fiction similarity:  [[0.18793815]]
Non-Fiction similarity:  [[0.24376105]]

A Modest Proposal by Jonathan Swift (607)    ['Political satire, English', 'Religious satire, English', 'Ireland -- Politics and government -- 18th century -- Humor']
Fiction similarity:  [[0.02830998]]
Non-Fiction similarity:  [[0.48431265]]

Heart of Darkness by Joseph Conrad (508)    ['Psychological fiction', 'Imperialism -- Fiction']
Fiction similarity:  [[0.36236417]]
Non-Fiction similarity:  [[0.09613796]]

A Warning to the Curious and Other Ghost Stories by M. R.  James (425)    []
Fiction similarity:  [[0.11689119]]
Non-Fiction similarity:  [[0.19405797]]

A Tale of Two Cities by Charles Dickens (556)    ['War stories', 'Historical fiction', 'England -- Fiction']
Fiction similarity:  [[0.0930782]]
Non-Fiction similarity:  [[0.12418421]]

The Odyssey by Homer (248)    ['Epic poetry']
Fiction similarity:  [[0.04639429]]
Non-Fiction similarity:  [[0.3002144]]

Alice's Adventures in Wonderland by Lewis Carroll (748)    ['Fantasy fiction', 'Imaginary places -- Juvenile fiction', "Children's stories"]
Fiction similarity:  [[0.3130059]]
Non-Fiction similarity:  [[0.18933827]]

Japanese Girls and Women by Alice Mabel Bacon (404)    ['Women -- Japan -- Social conditions']
Fiction similarity:  [[0.13491642]]
Non-Fiction similarity:  [[0.34866852]]

Dracula by Bram Stoker (620)    ['Horror tales', 'Gothic fiction', 'Vampires -- Fiction']
Fiction similarity:  [[0.09649402]]
Non-Fiction similarity:  [[0.19946308]]

War and Peace by graf Leo Tolstoy (412)    ['Historical fiction', 'War stories', 'Aristocracy (Social class) -- Russia -- Fiction']
Fiction similarity:  [[0.17590179]]
Non-Fiction similarity:  [[0.27332434]]

Beyond Good and Evil by Friedrich Wilhelm Nietzsche (275)    ['Ethics', 'Philosophy']
Fiction similarity:  [[0.15141302]]
Non-Fiction similarity:  [[0.23496476]]

Les Misérables by Victor Hugo (217)    ['Historical fiction', 'Epic literature', 'Orphans -- Fiction']
Fiction similarity:  [[0.06488585]]
Non-Fiction similarity:  [[0.14127941]]

Essays of Michel de Montaigne — Complete by Michel de Montaigne (254)    []
Fiction similarity:  [[0.02146232]]
Non-Fiction similarity:  [[0.12770727]]

Frankenstein; Or, The Modern Prometheus by Mary Wollstonecraft Shelley (1963)    ['Science fiction', 'Horror Tales', 'Gothic Fiction', 'Scientists -- Fiction']
Fiction similarity:  [[0.10958128]]
Non-Fiction similarity:  [[0.17222267]]

The Kama Sutra of Vatsyayana by Vatsyayana (235)    ['Love', 'Sex']
Fiction similarity:  [[0.08646447]]
Non-Fiction similarity:  [[0.20152023]]

Walden, and On The Duty Of Civil Disobedience by Henry David Thoreau (341)    ['Biography', 'Solitude', 'Natural history -- Massachusetts -- Walden Woods']
Fiction similarity:  [[0.1117556]]
Non-Fiction similarity:  [[0.32351252]]

The Great Gatsby by F. Scott  Fitzgerald (510)    ['Psychological fiction', 'Married women -- Fiction', 'First loves -- Fiction']
Fiction similarity:  [[0.32229045]]
Non-Fiction similarity:  [[0.05365625]]

Kwaidan: Stories and Studies of Strange Things by Lafcadio Hearn (259)    ['Ghost stories', 'Paranormal fiction', 'Japan -- Social life and customs -- Fiction']
Fiction similarity:  [[0.1615097]]
Non-Fiction similarity:  [[0.21151352]]

The Prophet by Kahlil Gibran (282)    ['Prose poems, American', 'Mysticism -- Poetry']
Fiction similarity:  [[0.09554582]]
Non-Fiction similarity:  [[0.2847405]]

A Doll's House : a play by Henrik Ibsen (599)    ['Drama']
Fiction similarity:  [[0.36909798]]
Non-Fiction similarity:  [[0.12462879]]

Great Expectations by Charles Dickens (446)    ['Orphans -- Fiction', 'England -- Fiction', 'Revenge -- Fiction', 'Bildungsromans']
Fiction similarity:  [[0.23567179]]
Non-Fiction similarity:  [[0.14792575]]

Narrative of the Life of Frederick Douglass, an American Slave by Frederick Douglass (289)    ['Biography']
Fiction similarity:  [[0.14226636]]
Non-Fiction similarity:  [[0.3139574]]

The Yellow Wallpaper by Charlotte Perkins Gilman (503)    ['Psychological fiction', 'Married women -- Psychology -- Fiction']
Fiction similarity:  [[0.35056472]]
Non-Fiction similarity:  [[0.19185829]]

Siddhartha by Hermann Hesse (299)    ['Spiritual life -- Fiction']
Fiction similarity:  [[0.19157973]]
Non-Fiction similarity:  [[0.12734298]]

The Count of Monte Cristo, Illustrated by Alexandre Dumas (333)    ['Historical fiction', 'Revenge -- Fiction', 'Adventure stories', 'Pirates -- Fiction']
Fiction similarity:  [[0.05591396]]
Non-Fiction similarity:  [[0.11180107]]

The Adventures of Tom Sawyer, Complete by Mark Twain (238)    ['Humorous stories', 'Boys -- Fiction', 'Adventure stories', 'Bildungsromans']
Fiction similarity:  [[0.35497108]]
Non-Fiction similarity:  [[0.11962791]]

Anthem by Ayn Rand (218)    ['Science fiction', 'Psychological fiction', 'Love stories', 'Men -- Psychology -- Fiction']
Fiction similarity:  [[0.21538979]]
Non-Fiction similarity:  [[0.205314]]

Don Quixote by Miguel de Cervantes Saavedra (249)    ['Spain -- Social life and customs -- 16th century -- Fiction']
Fiction similarity:  [[0.01779339]]
Non-Fiction similarity:  [[0.03409424]]

Summer by Romain Rolland (361)    []
Fiction similarity:  [[0.2446959]]
Non-Fiction similarity:  [[0.1727797]]

Jane Eyre: An Autobiography by Charlotte Brontë (500)    ['Orphans -- Fiction', 'England -- Fiction', 'Young women -- Fiction', 'Love stories', 'Bildungsromans']
Fiction similarity:  [[0.04781731]]
Non-Fiction similarity:  [[0.20992048]]

We can see most of fiction categories were assigned properly. 

Only 13 of a total of 38 books were not classified correctly, yielding a 65.8% success rate.

The exceptions were the following:
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
