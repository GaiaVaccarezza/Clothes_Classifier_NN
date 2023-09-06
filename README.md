During the cource of Deep Learning for Computer Vision of my Master of Science three collegues of mine and I 
were asked to design and train a classifier model to label the type of clothing showed in the images of the **iMaterialist Challenge at FGVC 2017 dataset**.

After cleaning the training dataset from corrupted or uncorrelated images and augmenting them through transformation, we built several classifier models using **Convolutional Neural Networks** that recognise if an image is a dress, shoe, pants, or outerwear. 
We decided to use Convolutional Neural Networks because they are specialised in processing data that has a grid-line topology. Therefore, as the scope of the project is to classify images, 
that can be represented as a 2D grid of pixels, they are the best choice for our purposes
Since the dataset provided, together with the photo and the clothing label, other informations like gender or material, we used **Multi-task Learning**. We hypothesised that the information contained in the other variables
could help predict apparel class more accurately. Indeed, Multi-task learning (MTL) is a subfield of machine learning in which multiple tasks are learned jointly, exploiting
similarities and differences across tasks. That what is learned for each task may help other tasks be learned better. This can result in improved learning efficiency and prediction accuracy 
for the task-specific models when compared to the model that was only trained on the main task (apparel class).

We trained two types of CNNs.

The first model was called the "private” due to its layers that are private for each task. Therefore, Multi-task learning is not applied to it. We built it to make comparisons to models where MLT is applied.
The model starts with an input layer followed by three unstrided convolution layers with 8,16 and 32 filters and kernel size (4,4). All of them use ReLU as activation function and apply padding to keep as much information as the original input as possible. After each convolution layer, there is a max pooling layer of size (4,4) to reduce the number of parameters and control overfitting. The third max pooling layer outputs a 3D tensor that needs to be flattened to be accepted as input by the dense layer, so we turn it into a vector. Next, the model has a dropout layer with a rate of 0.5, which means that it randomly sets the 50% of the features to 0. The reason is that the model has a lot of parameters and this layer may reduce overfitting. After, there is a common 128-wide fully connected (dense) layer with tanh activation function followed by two dense layers that are specific for each task in the model.
The wideness of the latter is proportional to the number of labels that each task has. In particular, they are 16*(number of labels) and 8*(number of labels) wide with a ReLu activation function. The reason for this choice is that we believe that if a task has more labels, it requires more complexity than one with fewer labels. Finally, the model is completed by an n-wide output layer that uses soft-max as an activation function. These n values are the probabilities that the model assigns to each label. We predict the label by taking the one with the highest probability.

Insteas the "public" model has a structure identical to the previous one until the Dropout Layer. After it, the model is composed of two layers with ReLU activation functions, that are shared by all the tasks. 
They are 160 and 126 wide. After that, there are the same 128-wide and n-wide dense layers of the last model. Therefore, while the “private” or “task-specific” layers disappear, two “public” layers are introduced. 
The reason for this choice is to have a model that suits better for multi-task learning. Indeed, with this “public” layout, the randomised weights in the NN always converge to what the apparel class tells them to go. 
Moreover, every other task now has a less-than-random weights setup to tweak every time it gets the chance. With private layers, instead, tasks are left on their own with no help from the apparel class' tweaking. 

A more detailed description of the project and its results is in the report pdf.
The code is currently under revision. It will be published soon.

