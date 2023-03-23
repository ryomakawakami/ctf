# Mysterious Learnings 
## Category: ML | Difficulty: Easy

> One day the archeologist came across a strange metal plate covered in uncommon hieroglyphics. It looked like blueprints for some kind of alien technology. "What kind of magic is this?" He studied the plate more closely and was amazed by the advanced technology and incredible engineering they were using at a time like this. This could only lead him in him wanting to learn more...

[ml_mysterious_learnings.zip](ml_mysterious_learnings.zip)

---

The only thing we see in the zip is `alien.h5`. The challenge description talks about some blueprints, so that makes me think this file isn't training data but rather a model.

I looked up "load model from h5" and saw a bunch of results about [loading Keras models](https://www.tensorflow.org/guide/keras/save_and_serialize). Let's see if we get some matches for `keras` with `strings`.

```
> strings alien.h5 | grep keras
keras_version
keras_version
```

Looks promising. Let's load the model and see what happens.

```python
from tensorflow import keras
keras.models.load_model('alien.h5')
```

Output
```
SFRCe24wdF9zb
[some stuff here]
<keras.engine.sequential.Sequential at 0x7fbc0c1dd310>
```

Cool. The `SFRCe24wdF9zb` looks suspicious. My guess is that it's base64 encoded. Let's see if we can decode it.

```
> echo SFRCe24wdF9zb | base64 -d
HTB{n0tbase64: invalid input
```

Okay, looks like it's at least part of the flag. It says invalid input because the base64 isn't complete.

Now this is the part I got stuck on for a bit. We have no data, so we can't really try to get an output from the model or anything. Maybe we'll look at the architecture of the model and see if that helps us at all.

```python
model = keras.models.load_model('alien.h5')
model.summary()
```

Output
```
SFRCe24wdF9zb
Model: "19oNHJkX3RvX3V"
_________________________________________________________________
 Layer (type)                Output Shape              Param #   
=================================================================
 conv2d_3 (Conv2D)           (None, 30, 30, 32)        896       
                                                                 
 max_pooling2d_2 (MaxPooling  (None, 15, 15, 32)       0         
 2D)                                                             
                                                                 
 conv2d_4 (Conv2D)           (None, 13, 13, 64)        18496     
                                                                 
 max_pooling2d_3 (MaxPooling  (None, 6, 6, 64)         0         
 2D)                                                             
                                                                 
 conv2d_5 (Conv2D)           (None, 4, 4, 64)          36928     
                                                                 
 uZDNyc3Q0bmR9 (Lambda)      (None, 4, 4, 64)          0         
                                                                 
 flatten_1 (Flatten)         (None, 1024)              0         
                                                                 
 dense_2 (Dense)             (None, 64)                65600     
                                                                 
 dense_3 (Dense)             (None, 10)                650       
                                                                 
 lambda_1 (Lambda)           (None, 10)                0         
                                                                 
=================================================================
Total params: 122,570
Trainable params: 122,570
Non-trainable params: 0
_________________________________________________________________
```

Well, this looks promising. Let's append the model name `19oNHJkX3RvX3V` and the first lambda layer name `uZDNyc3Q0bmR9` onto the string we already have.

```
> echo "SFRCe24wdF9zb19oNHJkX3RvX3VuZDNyc3Q0bmR9" | base64 -d
HTB{n0t_so_h4rd_to_und3rst4nd}
```

Yeah, that's probably the flag: `HTB{n0t_so_h4rd_to_und3rst4nd}`
