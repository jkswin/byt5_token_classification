# BYT5 for Token Classification

This repo/notebook is for developing a general framework for multilingual token classification tasks. With a baseline task of POS tagging in English, it involves the development and implementation of the following:

- Preprocessing and postprocessing logic for mapping Conditional Generation to a Token Classification task
- A Custom loss function for encouraging predicitions to avoid a specified tag during fine tuning (useful if your tagset has an UNK style tag)
- Noisy Embedding Fine Tuning
- A classification metric evaluation procedure for unstructed model generations that is slightly robust to poorly structured output

The choice of ByT5 is due to the following:

- The ByT5 paper gives evidence that Byte-based models are particularly suited to tasks that operate below the token level. I would argue that token classification is one such task as it is essentially reasoning with incredibly small sequences.
- Byte models have also shown evidence to be more robust to noise. In a regular tokenizer, a wrong character can result in a completely different subword and therefore embedding. Since bytes are already the smallest unit, they cannot be purturbed in this way.
- The small version of the model is still exactly that - relatively small being only 300M params and ~1.2GB.
- One noted capability of the byte model is that by nature the vocab size is 100x smaller than what modern tokenizers usually produce. This allows the excess parameters to be used in the knowledge/reasoning parameters like attention and so a 300M param byte model actually has the hidden layer capacity of a larger model.
- To me, using bytes as tokens also makes the model more language agnostic since there is no potential language-bias in learning the tokenization patterns as with things like sentence piece.

Notes:

- You may notice that validation loss is consistently lower than training loss. I put this down to (from intuition not thorough exploration) dropout being used during training but not during validation.

- During initial testing, PEFT methods such as LORA drastically reduced performance, especially where giving the correctly structured output was concerned. I therefore opted for a full fine tune given that the model is already relatively small. In future, it is probably worth reevaluating PEFT as an option as it was only explored in a very very limited capacity here.
