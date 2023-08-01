
---


### Definiton clarification :what's the difference between the "Dependencies" and the "Requirements"？

"Dependencies" refer to external libraries or packages that your code relies on in order to function properly. "Requirements" typically refer to a specification of the minimum version of those dependencies that are necessary to run your code. In other words, "Requirements" tell the user what version of the "Dependencies" are needed for the code to work correctly.

### Why use Random Seed in Machine Learning?

We use random seed value while creating training and test data set. The goal is to make sure we get the same training and validation data set while we use different hyperparameters or machine learning algorithms in order to assess the performance of different models. 


### Recommended pre-reading materials
* [cs self-learning guide](https://csdiy.wiki) `// every newbie should scan through it. //
`
* [c++ tutorial from the cherno on youtube](https://youtu.be/3tIqpEmWMLI) ：`// it's a great tutorial for beginners // 
`
* [The difference between Module, Package and Library in Python](https://parathan.medium.com/the-difference-between-module-package-and-library-in-python-e876f79ab2d8
)
* [colab basics](https://sspai.com/post/52980) ` // how to use colab to "python" //
`
* [Quick Language Models using](https://www.youtube.com/watch?v=tL1zltXuHO8)  ` // quick start using models //`

* [100+ Computer Science Concepts Explained](https://www.youtube.com/watch?v=-uleG_Vecis) ` // basic terms concepts //
`
* [How I program C](https://www.youtube.com/watch?v=443UNeGrFoM)：  `// basics and principles for writing c // last time watched till 1:37:05 / 2:11:31`

* [How to be a git expert ](https://www.youtube.com/watch?v=hZS96dwKvt0)` // git basics and workflow tutorial //
`
* [Awesome c++ leetcode solutions and program principels](https://github.com/youngyangyang04/leetcode-master)   `// I was reading the part about programming style //
`
* [why git push fails and shows up"fatal: The remote end hung up unexpectedly"](https://stackoverflow.com/questions/15240815/git-fatal-the-remote-end-hung-up-unexpectedly") And [more](https://stackoverflow.com/questions/15240815/git-fatal-the-remote-end-hung-up-unexpectedly/64565533#64565533)



### how to better debug machine learning model

* Learn how to debug machine learning model ： [how to better debug machine learning model](https://neptune.ai/blog/debugging-deep-learning-model-training)

	Recap： Since the machine learning model can "seems to be good", but you have to test them if you ignore some detailed errors in the code. Just create tests that assert that the neural network architecture looks the way it is supposed to, checking the number of layers, the total number of trainable parameters, the value range of the output, and so on.
	
	
* Using "from jsonargparse import CLI" to automatically pass arguments into parameters

```python
Sure! Let's say that you define the parameters look like this:

** code **
def prepare(
    destination_path: Path = Path("data/alpaca"), 
    tokenizer_path: Path = Path("checkpoints/lit-llama/tokenizer.model"),
    test_split_size: int = 2000,
    max_seq_length: int = 256,
    seed: int = 42,
    mask_inputs: bool = False,  # as in alpaca-lora
    data_file_name: str = DATA_FILE_NAME
):..........

if __name__ == "__main__":
	CLI(main)
	
** code **

Then you can use the command line like this ：
python prepare_alpaca.py prepare --destination_path data/alpaca --tokenizer_path checkpoints/lit-llama/tokenizer.model --test_split_size 2000 --max_seq_length 256 --seed 42 --mask_inputs False --data_file_name alpaca_data_cleaned_archive.json

That means you can make "passing the arguments into the parameters" extremely flexible.

```

*  Use Ctrl + - (macOS) keyboard shortcut to navigate back to the last edited location in VScode.
*  [softmax - you have to know how does it work](https://deepai.org/machine-learning-glossary-and-terms/softmax-layer)
*  [what is the "temperature" of LLM and how to tweak it](https://blog.finxter.com/chatgpt-api-temperature/)
* `file=sys.stderr` is to separate error messages from regular output and redirect them to a separate file or stream for debugging purposes.


E.g.
` print(f"Memory used: {torch.cuda.max_memory_reserved() / 1e9:.02f} GB", file=sys.stderr)
`

* [how to make sure your data is proper](https://neptune.ai/blog/ml-model-debugging-and-tools) // [A Comprehensive Guide to Data Preprocessing](https://neptune.ai/blog/data-preprocessing-guide)
```
e
```








