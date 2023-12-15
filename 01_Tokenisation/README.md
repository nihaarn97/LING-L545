<h1> Practical 01a </h1>
<h1> Segmentation </h1>

I extracted around 10 paragrpahs of random text from the Wikipedia dump of the Maori language. This text contains 60 sentences over all the paragraphs. For sentence segmentation, I tried the Python Sentence Boundary Disambiguation (pySBD) and the functionalities of the Python Spacy library.<br>
Results are as follows -

<h2> Python Sentence Boundary Disambiguation </h2>
Since Maori is written in a similar style as English, I set the language of the segmenter to English.
<h3> Does it use regular expressions? </h3>
As listed in the paper that introduced this library, the segment stage identifies true sentence boundaries by bypassing unicode characters and segments text into sentences using a simple regex rule. Common rules encompass the main sentence boundary regexes; AM-PM regexes handle numerically expressed time periods; number regexes handle period/newline characters before or after single/multidigit numbers and additional rules handle quotation, parenthesis, and numerical references within the input text. The Standard rule set contains regex patterns to handle single/double punctuation, geolocation references, fileformat mentions and ellipsis in input text.
<h3> Which programming language is it written in? </h3>
pySBD is a direct port of Ruby's pragmatic segmenter that's written in Python
<h3> Does it use machine learning? </h3>
No, it does not use any machine learning in its implementation.
<h3> Is it described anywhere? </h3>
The link to the paper that introduces pySBD is - https://aclanthology.org/2020.nlposs-1.15.pdf
The link to the GitHub repository is - https://github.com/nipunsadvilkar/pySBD
<h3> Quantitative Evaluation </h3>
The pySBD code achieves an accuracy of 100%, segementing all 60 sentences perfectly.
<h3> Qualitative Evaluation </h3>
Since pySBD did not make any mistakes in segmentation, a qualitative evaluation is not required.

<h2> SpaCy Sentence Segmenter </h2>
Since Maori is written in a similar style as English, I load the pre-trained language models for English (en_core_web_sm & en_core_web_lg) to perform sentence segmentation.
<h3> Does it use regular expressions? </h3>
SpaCy's sentence segmentation functionality does not primarily rely on regular expressions. Instead, it employs a combination of rule-based heuristics and machine learning to determine sentence boundaries. The machine learning component allows the model to adapt to various contexts and languages.
<h3> Which programming language is it written in? </h3>
SpaCy is primarily written in Python
<h3> Does it use machine learning? </h3>
Yes, SpaCy's sentence segmentation involves machine learning. The specific algorithm used for training is typically a convolutional neural network (CNN) architecture. SpaCy is trained on a large annotated corpus to learn patterns and features that help identify sentence boundaries. The training corpus includes diverse texts to ensure generalization.
<h3> Is it described anywhere? </h3>
The official documentation for SpaCy, including information about its sentence segmentation functionality and models can be found on the spaCy website https://spacy.io/.
<h3> Quantitative Evaluation</h3>
Using the small English model (en_core_web_sm), the number of sentences obtained are 165, which is an error rate of approximately 175%. Sentences correctly segmented are 12 indicating an accuracy of 20%.<br>
Using the large English model (en_core_web_lg), the number of sentences obtained are 119, which is an error rate of approximately 99%. Sentences correctly segmented are 16 indicating an accuracy of 26.67%.<br>
<h3> Qualitative Evaluation </h3>
It is difficult to evaluate where the SpaCy sentence segmenter is going wrong as it seems to randomly split a complete sentence into smaller chunks without any recurring pattern. A mistake that SpaCy is making for the text I have entered is, not segmenting a sentence when it ends with ':' and continuing to include some words from the next sentence after the colon. SpaCy is able to correctly identify the ends of a sentence by appropriately splitting at the '.', however since overall accuracy is low it does not make a significant difference.

<h1> Tokenisation </h1>
<h2> Implementation of maxmatch </h2>

def maxmatch(text, dictionary):<br>
    result = []<br>
    while text:<br>
        found = False<br>
        for i in range(len(text), 0, -1):<br>
            firstword = text[:i]<br>
            remainder = text[i:]<br>
            if firstword in dictionary:<br>
                result.append(firstword)<br>
                text = remainder<br>
                found = True<br>
                break<br>
        if not found:<br>
            result.append(text[0])<br>
            text = text[1:]<br>
    return ' '.join(result)<br>


<h2> Instructions on how to run it </h2>
The jupyter notebook in this contains all the code. It runs in the following manner -
<ol>
	<li>Generation of surface forms dictionary using the ja_gsd-ud-train.conllu file</li>
	<li>Creating a reference.txt file containing 7 lines of sample text using the ja_gsd-ud-test.conllu file</li>
	<li>Defining the maxmatch function</li>
	<li>Running the maxmatch function on the sample text from reference.txt to create the hypothesis.txt file</li>
</ol>

Once the reference.txt and hypothesis.txt files are generated we can use the terminal to run the command 
python wer.py reference.txt hypothesis.txt 
in order to evaluate the maxmatch algorithm performance. The output of this is shown below.

<h2> Brief description of performance </h2>
python wer.py reference.txt hypothesis.txt<br>
REF: これ に 不快 感 を 示す 住民 は い まし た が , 現在 , 表立っ    て 反対 や 抗議 の 声 を 挙げ て いる 住民 は い  ない よう です 。 幸福 の 科学 側 から は , 特に どう し て ほしい と いう 要望 は いただ い て い ませ ん 。 星取り     参加 は 当然 と さ れ , 不 参加 は 白眼   視 さ れる 。 室長 の 対応 に は 終始 誠実 さ が 感じ られ た 。 多く の 女性 が 生理   の こと で 悩ん で い ます 。 先生 の 理想 は 限 りなく    高い 。 それ は 兎も角     , 私 も 明日 の 社説   を 楽しみ に し て おり ます 。
HYP: これ に 不快 感 を 示す 住民 は い まし た が , 現在 , 表   立っ て 反対 や 抗議 の 声 を 挙げ て いる 住民 は い  ない よう です 。 幸福 の 科学 側 から は , 特に どう し て ほしい と いう 要望 は いただ い て い ませ ん 。 星   取り  参加 は 当然 と さ れ , 不 参加 は 白  眼 視 さ れる 。 室長 の 対応 に は 終始 誠実 さ が 感じ られ た 。 多く の 女性 が 生  理 の こと で 悩ん で い ます 。 先生 の 理想 は 限 り   なく 高い 。 それ は 兎   も 角 , 私 も 明日 の 社  説 を 楽しみ に し て おり ます 。<br>
EVA:                                     S   I                                                                                                     S   I                         S  I                                                    S  I                              S    I            S   I I            S  I<br>
WER: 12.40%<br>

A WER of 12.4% suggests that approximately 12.4% of the words in the hypothesis differ from the reference. The low WER indicates good performance.
