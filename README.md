# Ultrasound Nerve Segmentation

###Description
This was a competition on kaggle to identify nerve structures in ultrasound images. The sponsor was working to improve pain management through the use of indwelling catheters that block or mitigate pain at the source and this would improve catheter placement. 

### Evaluation
The results of the model are evaulated on the Dice coefficent to compare pixels of generated masks of segmentation to a ground truth mask.
$$\frac{2 * |X \cap Y|}{|X| + |Y|}$$

The X is predicted and Y is ground truth, the optimal value is 1.

### Approach and Problem

####Model
Used neural network based on U-net architecture ([Source](https://github.com/jocicmarko/ultrasound-nerve-segmentation))implemented in Keras. The images are scaled down to 512 pixels.

####Data Augmentation
Involved modification of the Kaeras image generator class. This allowed for images and masks to be rotated, skewed, etc. to help decrease overfitting and provide a larger dataset.

####Data Preprocessing
One of the most imporant parts of building a model is the pre-processing. So the first thing, after getting a working submission was to start examining the data for ways to add new features or alter to dataset to give better results. There seemed to be a lot of noise in the images although it scales down to 512 pixels, there would still be some noise visible so I tried using various techniques including ZCA Whitening and anisotropic diffusion to reduce the noise and make the nerves stand out more. 

![Image of Nerves and Anisotropic Diffusion](https://github.com/cpueschel/Ultrasound-Nerve-Segmentation/blob/master/despeckle.png?raw=true)

As well as some more extreme attempts to completely isolate the nerves which appeared to lower the accuracy due to information loss. 

![Isolation Image of Nerves and Anisotropic Diffusion](https://github.com/cpueschel/Ultrasound-Nerve-Segmentation/blob/master/isolation.png?raw=true)

###Execution

Most of my processing and training was done on Amazon EC2 using spot-instances to keep the price very low. My processes were killed a few times since my maximum bid was lower but you can quickly start it up again and load the data that was generated back in to continue. 

Locally, I was seing score with the Dice of around 0.77 even with the data augmentation. Once uploaded to the leaderboard there was a massive discrepancy in the values. Unfortuantly, I started this very late in the last two weeks of the contest and was not able to correct with cross-validation for the overfitting of the training dataset. The largest jump was once the image pre-processing was applied, once the noise was removed from the images, the score on the public leaderboard jumped.
