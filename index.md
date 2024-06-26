# Phonotactic Calculator for Czech

To find out more about our calculator, read the concise paragraphs below or our detailed article:<br>__Čechová, P., Cilibrasi, L., Henyš, J. & Čecho, J. (2023). [Introducing a phonotactic probability calculator for Czech](https://asjournals.lib.cas.cz/naserec/article/uuid:35b99c0d-ef22-4a45-9e31-013a0731a48a). *Naše řeč*, 106(1), 72-83__.

To calculate phonotactic probability, download the python script and related files below. If necessary, do read instructions on how to install python on your computer (so far available only for Windows). 
To run the script based on your needs, have a look at our example commands, otherwise, read the README.txt file.

[Download PhonoCalc (ZIP archive)](https://github.com/phonocalc/phonocalc.github.io/raw/968fbfffd890d0c3c7772040d30f376bf04c4a57/PhonoCalc.zip)

## What it is 

### What is phonotactic probability?
Phonotactic probability refers to the frequency with which phonological segments and sequences of phonological segments occur in words in a given language (Vitevich &
Luce, 2004).

### What is it good for?
It has been shown that estimates of phonotactic probability of words are important
in language processing and language acquisition (Jusczyk, Luce & Charles-Luce,
1994; Mattys & Jusczyk, 2001; Pitt & McQueen, 1998). For example, words with high
phonotactic probability are processed faster by native speakers in same-different
tasks (Luce & Large, 2001) and pseudowords with high phonotactic probability are
judged as more word-like by adults (Vitevitch, Luce, Charles-Luce & Kemmerer, 1997).

### How is it counted?
In the original calculator for English (https://calculator.ku.edu/phonotactic/about), the phonotactic probability is estimated based on two measures: positional segment frequency and position specific bisegment frequency. Our script can be executed based on any n-gram.
The unigram positional probability is calculated by searching the selected frequency list for all words that contain a given segment in a given position. For instance, if one looks at the word srna, /srna/ (roe deer), it means searching for every word with /s/ in the first position, /r/ in the second one, /n/ in the third one and /a/ in the fourth one. Then, for every segment, log (base 10) values of  all the frequencies of the respective words are summed and divided by a sum of log values of frequencies of all words which contain any segment in the same position (in the case of our example /srna/, for /s/ it means all words that are at least one-segment long, for /r/ all words that are at least two-segment long, for /n/ all words that are at least three-segment long and so forth). The arithmetic mean of these values for every segment in a word serves as a final phonotactic probability estimate. The same applies to bigram or n-gram position-specific frequency, the only difference being that the procedure works with a sequence of two or n segments in a given position instead of one. For position-specific bigram frequency of the word srna, this would mean searching for every word with /sr/ in the first position, /rn/ in the second one and /na/ in the third one and then execute the calculation as described in the case of the unigram.

## How the script is made

### What data is the script based on?
The python script provides estimates of phonotactic probability based
on frequency of word tokens in a synchronous reference corpus of contemporary
written Czech SYN2015 (Křen & Cvrček et al., 2015), SYN2020 (Křen & Cvrček et al., 2020) or ORAL, a spoken corpus of informal conversations (Kopřivová & Lukeš et al., 2017). The script can also be executed based on a custom frequency list. The words are automatically transcribed into IPA using the CorPy library (Lukeš, 2016). 

### The script can count both orthotactic and phonotactic probability, but you are marketing only the phonotactic one.
  
When creating the script, we first focused solely on orthotactic probability (i.e. and estimate that refers to the frequency with which letters and sequences of letters occur in words in a given language with alphabetical writing) because it was easier and faster to calculate (no need to transcribe to IPA). We later added the transcription feature, hence calculating phonotactic probability, but we decided that orthotactic probability may be useful to researchers analysing processing of written language, so we kept it there. The calculation works the same, only the n-gram constitute graphemes rather than phonemes.
  
### What is the input and output of the script?
The input for the script is any existing or non-existing word or an entire list of (non-)words, the output gives the phonotactic probability estimates.

## How to use the script

Download the needed files [here](https://github.com/phonocalc/phonocalc.github.io/raw/968fbfffd890d0c3c7772040d30f376bf04c4a57/PhonoCalc.zip).

If you are familiar with python, install required libraries specified in the requirement.txt file. 

If you are not familiar with python, read how to install it and the required libraries to your computer [here](https://github.com/phonocalc/phonocalc.github.io/raw/1eba3c5574bfaf97dd5020b8e94325340f9c34c9/PhonoCalc_tutorial_en.pdf). (so far only for Windows). 

### Commands to run the script with:

The basic command counts orthographic probability (because it requires shorter time) based on position specific bisegment frequency and SYN2015. This example command asks for the orthographic probability estimate of the pseudoword “mrát”:
  
```
python3 phon_calc.py --word mrát
```
  
Multiples words or pseudowords can be put into one command:
```
python3 phon_calc.py --word mrát --word brát
```

To count phonotactic probability, add `--transcribe`.
```
python3 phon_calc.py --word včela --transcribe  
```

To count orthotactic probability based on positional segment frequency, add `--ngram 1`:
```
python3 phon_calc.py --word čmelák --ngram 1
```

If you want to compute phonotactic probability based on positional segment frequency, add both `--ngram 1` and `--transcribe`
```
python3 phon_calc.py --word čmelák --ngram 1 --transcribe
```

If you want to calculate based on specific trisegment frequency:
```
python3 phon_calc.py --word čmelák --ngram 3 --transcribe
```

Calculate based on SYN2020:
```
python3 phon_calc.py --database syn2020_word_utf8.tsv --word blik
```
  
Calculate based on ORAL:
```
python3 phon_calc.py --database oral_word_utf8.tsv --word kuk
```

If you want to calculate a reversed probability, add `--reverse`:
```
python3 phon_calc.py --word čmelák --reverse
```

Input consisting of higher tens or hundreds of (non)words can be loaded from a file:
```
python3 phon_calc.py --words-file input_file.csv
```

### How to calculate based on your own frequency list?
You need to save your frequency list in the same folder as the script is, preferably in a .tsv format. Then you need to specify its name and the column number for words and frequency in the command:
```
python3 phon_calc.py --database your_frequency_list.tsv --word hups --database-word-index 0 --database-frequency-index 1
```

If you want to transcribe your frequency list into IPA and print it:
```
python3 phon_calc.py --transcribe-database
```

### How to save the final estimates?
And finally, to save the final estimates to a file, add `> name_of_the_file.txt` to a command:
```
python3 phon_calc.py  --word blik --klik --transcribe --reverse > bi_phon_prob_reversed.txt
```

### How to cite?
When using our calculator, please cite the following article:<br>
__Čechová, P., Cilibrasi, L., Henyš, J. & Čecho, J. (2023). Introducing a phonotactic probability calculator for Czech. *Naše řeč*, 106(1), 72-83__.
  
### Authors:

**Petra Čechová**<br/>
petra.cechova(at)ff.cuni.cz<br/>
Faculty of Arts, Charles University

**Jaroslav Čecho**

**Jan Henyš**<br/>
jan.henys(at)ff.cuni.cz<br/>
Faculty of Arts, Charles University
 
