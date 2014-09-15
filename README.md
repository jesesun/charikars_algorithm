Detection of near-duplicate documents
==================

When you have lots of digital documents you would like to know how many duplicates or near-duplicates exist on your storage devices, because you want to clean up space, detect plagiarism or avoid include documents with different versions. 
Duplicate documents:
When you need to find documents that are exact copies, these are easy to find - just calculate a checksum and compare with others.
Near-duplicate: documents differ from each other in a very small portion, for example a document life cycle, you want to find documents which basically are different versions of the same document.

This software is near-duplicate detection system that uses Charikar's fingerprinting technique and Rabin-Hash function.

##Charikars algorithm implementation

#How can detect near-duplicates?

To detect near-duplicates this software uses the Charikar's fingerprinting technique, this means characterizing each document with a unique 64-bit vector, like a fingerprint.
To determine whether two documents are Near-duplicates, we have to compare their fingerprints.

To do this we use two algorithms, the algorithm developed by Moses Charikar and the Hamming distance algorithm, which allows us to measure the similarity between two vectors of n bits.

#What is Charikar's algorithm?

- Characterization of the document
- Apply hash functions to the characteristics
- Obtain fingerprint
- Apply vector comparison function:
     Are (Doc1, doc2) near-duplicate?  Hamming-distance (fingerprint (doc1), fingerprint (doc2)) = k

#What means characterize documents?

Characterizing Document is what things makes a document different from the others, for example number of words, number of paragraphs, punctuation, sentences, etc ...

It is up to us to define what is the most appropriate feature to generate the fingerprint.

One thing that characterize is  a technique known as shingling. This technique consist to split documents in Shingles, set of words grouped in n n, also called n-gram grouping.

For example, shingling in 3-grams:
     Phrase: detecting near duplicates of documents
     3-grams: (detection of documents, documents near, near duplicates documents).

To further characterize the document, we remove stop-words (words without meaning) and do stemming on every word (extract the root of the word), previously we will detect the language.

So the above example would be:
     Phrase: detecting near duplicates of documents
     Stop-words: detecting near duplicates documents
     Stemming: near duplicate document detection
     3-grams: Document detect near, near duplicate document

A singly extracted features apply a hash function to each of them.

We will apply the Rabin-Hash function, exists other functions.

There are many hash function that can be used.
We decided to use the hash-Rabin because it very quickly.

For the phrase "Detecting near duplicates" we get the following 64-bit hash.

Detecting - 0000000000000000000000000000000000000000000110100111110100001011

Near - 0000000000000000000000000000000000000000000011110000000000010111

Duplicates - 0000000000000000000000000000000001111101100000110110100101111111


#How do you calculate a fingerprint?

We start from a 64-bit vector with all 0, for each hash obtained from the documen we have to apply the following function:
If the bit vector word 1 increases resulting vector 1, but (<= 1) decreases in the 1.
example:

Initial vector    0  0 0  0 0  0  0 0 0 0 0 0 0  0  0  0  0  0  0 0 0 0

Detecting -       0  1 1  0 1  0  0 1 1 1 1 1 0  1  0  0  0  0  1 0 1 1

Result Vector1 -  1 -1 1 -1 1 -1 -1 1 1 1 1 1 1 -1 -1 -1 -1 -1 -1 1 1 1


Result Vector1 -  1 -1 1 -1 1 -1 -1 1 1 1 1 1 1 -1 -1 -1 -1 -1 -1 1 1 1

Near -            0  0 1  1 1  1  0 0 0 0 0 0 0  0  0  0  0  1  0 1 1 1

Result Vector2 -  1 -2 2  0-2  2  0 0 0 0 0 0 0 -2 -2 -2 -2 -1  0 2 0 2


Result Vector2 -  1 -2 2 0 -2 2 0 0 0 0 0 0 0 -2 -2 -2 -2 -1 0 2 0 2

Duplicates -      1  1 0 0  0 0 0 1 1 0 1 1 0  1  0  0  1  0 1 1 1 1


Result Vector3.   2 -1 1 -1 1 -1 1 -1 1 1 1 -3 -3 -3 -3 -1 -1 1 1 0 3 3


Once you have applied all the characterizations in hash format, generate the fingerprint as follows:
If the resulting vector is negative or 0, the fingerprint in that position is 0, otherwise it would be 1.
In this example the fingerprint be as follows:


Result Vector3.   2 -1 1 -1 1 -1 1 -1 1 1 1 -3 -3 -3 -3 -1 -1 1 1 0 3 3

Fingerprint       0  1 1  0 1  0 0  1 1 0 1  1  0  1  0  0  0 0 1 1 1 1


#Hamming distance.

Algorithm to calculate the similarity between two documents, there are more, but we'll use this.
Basically what this algorithm does is: given two vectors of n bits, several changes must be made equal to one another, this number will be called K-Hamming distance.

#Are two document Near-duplicates ?

We consider that two documents are near-duplicates when the K-Hamming distance resulting from applying the fingerprint is <= 3.

##Usage

>dedup.jar [file1] [file2]  max [max ngrams] min [min ngrams]

>dedup.jar text_sample_2.txt text_sample_1.txt  max 3 min 3
