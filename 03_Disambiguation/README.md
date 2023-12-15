<h1> Improve perceptron Tagger </h1>
I tried various combinations of feature extractions like - 
<ul>
  <li>Add features based on the context of the current word.</li>
  <li>Add features based on prefixes and suffixes of various lengths.</li>
  <li>Incorporating information about the current and previous tags.</li>
  <li>Introduce features based on the surrounding words and tags.</li>
</ul>
Following is the code for these feature extractions -

```python
add('i+1 suffix', word[-2:]) #Added Feature
add('i+2 suffix', word[-1:])  #Added Feature
add('i-1 suffix', word[-4:])  #Added Feature
add('i-2 suffix', word[-5:])  #Added Feature
add('prefix_2', word[:2]) #Added Feature
add('prefix_3', word[:3]) #Added Feature
add('prefix_4', word[:4]) #Added Feature
add('i-1 tag+i+1 tag', prev, context[i+1]) #Added Feature
add('i-2 word+i word', context[i-2], context[i]) #Added Feature
add('i-1 tag unigram', prev) #Added Feature
add('i-2 tag unigram', prev2) #Added Feature
add('i-2 tag+i-1 tag bigram', prev2, prev) #Added Feature
add('i-1 word+i word', context[i-1], context[i]) #Added Feature
add('i+1 word+i word', context[i+1], context[i]) #Added Feature
add('i-1 tag+i word+i+1 tag', prev, context[i], context[i+1]) #Added Feature
add('i-2 morph suffix', context[i - 2][-3:])  #Added Feature
add('i-3 morph word', context[i - 3])  #Added Feature
add('i-3 morph suffix', context[i - 3][-3:])  #Added Feature
add('i+2 morph suffix', context[i + 2][-3:])  #Added Feature
```

Testing out various combination of features only gave me a small improvement of 96.06% at the most.<br>
I also tried by playing around with some features that are already included in the _get_features() function of tagger.py but it did not have any improvement on the performance. <br>
An output of the highest accuracy I achieved is included in the folder. <br>
