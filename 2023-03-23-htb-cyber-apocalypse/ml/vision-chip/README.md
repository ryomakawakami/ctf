# Vision Chip
## Category: ML | Difficulty: Medium

> In preparation for your mission, you've been fitted with a new visual cortex chip that is able to recognize common objects on Earth. To verify it's working correctly, run through a diagnostic test to see if you can distinguish people from large man-made constructions.

[ml_vision_chip.zip](ml_vision_chip.zip)

---

From the challenge description, it seems like we're going to get a pretrained model and use it on a test dataset to distinguish people from large man-made construction.

Let's unzip all the files.

```
test_X/0.png ...  test_X/8191.png
challenge.ipynb
meta
model.py
state_dict.pt
```

Looks like `test_X` contains 8192 images that we will be putting through the model to get the flag. We'll probably be working in `challenge.ipynb`. Not really sure what `meta` is. `model.py` is the model definition and `state_dict.pt` is probably the pretrained weights for the model.

See [challenge.ipynb](challenge.ipynb) for the rest of the writeup.
