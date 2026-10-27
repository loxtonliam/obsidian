#DS141 
#DS-Topic-7
#Text-Mining

# Terminology
**Document**
	one individual unit of data
	eg: emails, blog posts, tweets
**Terms**
	Individual components of a document (tokens)
**Corpus**
	collection of documents, ie: data set
**Document-Term-Matrix(DTM)**
	data representation with rows representing documents and columns representing terms
	structure: 
	![[DS Week 7 Notes - DTM.png]]
**Term-Document-Matrix(TDM)**
	a transpose of a **DTM**, rows = terms and columns = documents
	structure: 
	![[DS Week 7 Notes - TDM.png]]

# Text Representations
## Binary Bag of Words
>[!note] every document is treated as a collection of individual words. Grammer, word order, sentence structure and punctuation are ignored

- each word in each document is therefore treated as a potentially important keyword
**Advantages**
- straightforward
- shown to work well in practice

- each word is either given a value of 1 or 0 (hence binary)
	- ignores if a word occurs multiple times in a document => does not value repeating words as more important (standard bag of words)

**Example DTM**
![[DS Week 7 Notes - Binary DTM.png]]
where 1 shows that a word is in the document

## Bag of Words
- similar to **binary bag of words** but values word frequency
- example DTM:![[DS Week 7 Notes - bag DTM.png]]
note Data = 2 for doc_id =2
this is the raw counts

- DTM with normalised term frequencies
	- in proportion to document length
![[DS Week 7 Notes - normalised DTM.png]]

## TFIDF
>[!Definition] Term Frequency-Inverse Document Frequency

**Term frequency**
	measures how prevelant a term is in a single document
- terms should not be too rare or too common
	- this can be elimanted by imposing upper and lower limits on term frequencies
**IDF**
- Distribution of a term over an entire **corpus** can be calculated using the inverse document frequency of a term
- interpreted as the 'boost' a term gets for being rare
- calculated at a corpus level, not used on its own as a text representation
$$IDF(t) = log_2 \left( \frac{\text{Total number of documents in corpus}}{\text{No. of documents containing term t}} \right)$$
- example:
![[DS Week 7 Notes -IDF.png]]

**TFIDF**
$$TFIDF(t,d) = TF(t,d)*IDF(t)$$
> where t -> term, d -> document

- calculated at a document level => can be used a text representation
![[DS Week 7 Notes - TFIDF.png]]

## n-grams
>[!info] counts n groups of words

- bi-gram = a pair of words
	- data science is my favourite => data_science , science_is, is_my, my_favourite
**Advantages**
- easy to generate
- no linguistic knowledge required
**Disadvantages**
- greatly increase the size of the attribute set
- many n-grams will be rare
	- leads to sparse data matrices

## Named entity extraction
>[!info]  words in sequence have a different meaning to individual words

- ie: 'Game' 'of' 'thrones' and 'Game of Thrones' have different meanings
# Visualising text data
**Word clouds**
- popular way to visualise text data
![[DS Week 7 Notes - Word cloud.png|200]]
- provide visual representations of text that are easy to interpret

# Text Pre-processing
## Case Normalisation
- in most text represenations 'data' and 'DATA' are counted as different
- thus we will convert all words to lower case to avoid this

## Stemming
- words like 'learned' and 'learning' all stem from the same word 'learn'
- transform all these words into their stem
## Stopwrods
- these words are typically removed during pre-processing
	- words such as 'the', 'and'
## Other issues
- how numbers and symbols should be treated
- removal of punctuation and excess white space
- dealing with abbreviations and contractions

# Text mining in R
**tm package**
![[DS Week 7 Notes - tm package.png]]

**creating a corpus**
- done using the VCorpus() function
	-  V = volatile => corpus is stored in memory rather than on disk
- mycorpus <- VCorpus(VectorSource(text_example))

![[DS Week 7 Notes - DTM-1.png]]