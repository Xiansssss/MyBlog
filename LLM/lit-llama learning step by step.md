title: lit-llama learning step by step(it tormented me but now you can relax due to this article)
date: 2023-6-08
tags:
- LLM

categories: 
- LLM

---

## read first to know the rough stuff

Start from this article: https://lightning.ai/pages/community/tutorial/accelerating-llama-with-fabric-a-comprehensive-guide-to-training-and-fine-tuning-llama/

Repo that mentioned in the article : https://github.com/Lightning-AI/lit-llama/blob/main/scripts/prepare_alpaca.py

downlaod the model weights and parameters ：https://github.com/Lightning-AI/lit-llama/blob/main/howto/download_weights.md    https://huggingface.co/nyanko7/LLaMA-7B/tree/main

Bind your server repo to the github repo: https://levelup.gitconnected.com/fix-password-authentication-github-3395e579ce74



## project retrospective

### Trick to upload the big dataset

Add the chrome extension "CurlWget", and upload your data into OneDrive from microsoft（at least I tried it）and open the OneDrive on the webpage, choose your data in the web OneDrive and click the download button, then the extension I mentioned will capture the curl/wget information, now you can cancel the download process since it doesn't matter anymore. Afterwards, copy the whole information from the extension to your server/local terminal, paste it, and execute it. Everything's done, you will be over the moon when you see the upload speed up to 20Mb/s（depends on your network）.

### autodl (A server rent platform) data disk/ system disk 

when you try to upload the data just let it goes to the data disk since the storage of it can hold it, at least 50G

In addition, I would say open a terminal page to do the "cd" stuff will save your time af.

### Requirements(**cking important) 

lit-llama is compatible with pytroch>=2.0.0 IF NOT, the traceback will tell you the lack of torch.util.device module(roughly like this name, but may have some _ or - between them)

This one is good enough for me ：https://zhuanlan.zhihu.com/p/634963293 

My version here：

print(torch.__version__)	2.0.0+cu117
print(torch.version.cuda)		11.7
print(torch.cuda.is_available())	True

### error: pytorchstreamreader failed reading zip archive: failed finding central directory 

It's annoying, but just make sure the file is downloaded completely and then everything will be okay. If you already download that just use the "ll -lh" to check the file size whether it equals to the original one. If not, just delete it and reupload it. 



### Run Test

Cd root/lit-llama/    

then  run : 

```Python
!python generate.py --prompt "Hello, my name is"
```

it will returns:

```python
Loading model ..
Time to load model: 23.55 seconds.
Global seed set to 1234
Tell me the latest news about web3 and ReadON app?
The ReadON application has been updated and is now compatible with version 1.0.5.2285 of Web.
This application is an easy to use, high-quality, and free tool that allows you to
Time for inference 1: 2.06 sec total  24.22 tokens/sec
Memory used :13.54GB
```

IF everything works, the terminal returns things like these, it demonstrates your requirements have been downloaded correctly.



## Tune on your dataset

With only a few modifications, you can prepare and train on your own instruction dataset.

1. Create a json file in which each row holds one instruction-response pair. A row has an entry for 'instruction', 'input', and 'output', where 'input' is optional an can be the empty string if the instruction doesn't require a context. Below is an example json file:

```
   [
       {
           "instruction": "Arrange the given numbers in ascending order.",
           "input": "2, 4, 0, 8, 3",
           "output": "0, 2, 3, 4, 8"
       },
       ...
   ]
```

   ​

2. Make a copy of `scripts/prepare_alpaca.py` and name it what you want:

   ```
   cp scripts/prepare_alpaca.py scripts/prepare_mydata.py
   ```

   ​

3. Modify `scripts/prepare_mydata.py` to read the json data file.

4. Run the script to generate the preprocessed, tokenized train-val split:

   ```
   python scripts/prepare_mydata.py --destination_path data/mydata/
   ```

   ​

5. Run `finetune/lora.py` by passing in the location of your data (and optionally other parameters):

   ```
   python finetune/lora.py --data_dir data/mydata/ --out_dir out/myexperiment
   ```



## Improve the performance

To test the fine-tuned model, using the code below:
```
python generate/adapter.py \
    --prompt "Recommend a movie to watch on the weekend." 
```

And the result totally sucks.

So "improve the dataset" hit my mind first. FYI, you can use your fliter methods to process your dataset.

### 1.delete the image description 

```
lines = your_content.split("\n")
filtered_lines = [line for line in lines if "![img](" not in line]
```
### 2.delete the unrelated URL/emojis/....

**re** is a module which can use regular expression pattern to match and remove what you want.

```
import re

# Open the input file
with open('your_data_path', 'r') as f:
    text = f.read()

# Define the regular expression pattern to match emojis, URLs, ">_", "<3"
pattern = re.compile(u'[\U0001F600-\U0001F64F\U0001F300-\U0001F5FF'
                     u'\U0001F680-\U0001F6FF\U0001F1E0-\U0001F1FF]'
                     u'|[hH]ttps?\\S+|>_+|<3')

# Remove all text that matches the pattern
filtered_text = pattern.sub('', text)

# Write the filtered text to an output file
with open('your_output_path', 'w') as f:
    f.write(filtered_text)
```
### 3. dig into the model part
