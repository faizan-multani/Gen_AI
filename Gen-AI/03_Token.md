# Token :
- A token is a piece of text.

## For Example :
- A word

- A part of a word

- even punctuation

# Tokenization :
- Tokenization is the process of **converting raw text into tokens** — pieces that the model can understand.

- Converting the user input to set of numbers.

- Once tokenized, these tokens are converted into numbers (called token IDs) which the model actually works with.

## Why Tokenization Matters :
- Computers don’t understand text — they understand numbers.

- Tokenization is how we bridge the gap between text and numbers.

```
text = 'i love programming!'

converted token =       ["I", " love", " programming", "!"]
converted into numbers = 40,   453 ,     18327,        0

```

## Creating Tokens :
### step-1 :
- Create virtual environment.
- run this command in your current directory.
```
#bash/terminal
python -m venv myvenv
```

## step-2 :
- Activate venv.
```
source myvenv/bin/activated
```

## step-3 :
- install tiktoken.
```
pip install tiktoken
```

## create requirements.txt file :
```
pip freeze requirements.txt
```

## Example :
```
import tiktoken

enc = tiktoken.encoding_for_model('gpt-4o')

text ='hey there! i am faizan khan'
tokens =enc.encode(text) # to encode

print(tokens)

decode_tokens =enc.decode([25524,77827,19,9087,5612,1234]) # decoding

#Assuming OUTPUT :
# Encoded :
[25524,77827,19,9087,5612,1234]

# Decoded :
hey there! i am faizan khan
```

