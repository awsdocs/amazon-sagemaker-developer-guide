# Use TensorBoard in Amazon SageMaker Studio<a name="studio-tensorboard"></a>

 The following doc outlines how to install and run TensorBoard in Amazon SageMaker Studio\. 

## Prerequisites<a name="studio-tensorboard-prereq"></a>

This tutorial requires an Amazon SageMaker Studio Domain\. 

## Install TensorBoard<a name="studio-tensorboard-install"></a>

1. Launch Studio\. 

1. Click on the  `Amazon SageMaker Studio` button on the top left corner of Studio to open the Amazon SageMaker Studio Launcher\. This launcher must be opened from your root directory\. 

1. In the Launcher under `Utilities and files`, click `System terminal`\. 

1. From the terminal, run the following commands\. 

   ```
   pip install tensorboard 
   tensorboard --logdir ./logs/fit
   ```

## Set Up `TensorBoardCallback`<a name="studio-tensorboard-setup"></a>

1. In the Amazon SageMaker Studio Launcher under `Notebooks and compute resources`, select the `TensorFlow 2.3 Python 3.7(optimized for CPU)` Studio Image\. 

1. Launch a notebook\. 

1. Import the required packages\. 

   ```
   import os 
   import datetime 
   import tensorflow as tf
   ```

1. Create your Keras model\. 

   ```
   mnist = tf.keras.datasets.mnist
   
   (x_train, y_train),(x_test, y_test) = mnist.load_data()
   x_train, x_test = x_train / 255.0, x_test / 255.0
   
   def create_model():
     return tf.keras.models.Sequential([
       tf.keras.layers.Flatten(input_shape=(28, 28)),
       tf.keras.layers.Dense(512, activation='relu'),
       tf.keras.layers.Dropout(0.2),
       tf.keras.layers.Dense(10, activation='softmax')
     ])
   ```

1. Create a directory for your TensorBoard logs 

   ```
   LOG_DIR = "logs/fit/" + datetime.datetime.now().strftime("%Y%m%d-%H%M%S")
   ```

1. Run training with TensorBoard\. 

   ```
   model = create_model()
   model.compile(optimizer='adam',
                 loss='sparse_categorical_crossentropy',
                 metrics=['accuracy'])
                 
                 
   tensorboard_callback = tf.keras.callbacks.TensorBoard(log_dir=LOG_DIR, histogram_freq=1)
   
   model.fit(x=x_train,
             y=y_train,
             epochs=5,
             validation_data=(x_test, y_test),
             callbacks=[tensorboard_callback])
   ```

## Launch TensorBoard<a name="studio-tensorboard-launch"></a>

1. To launch TensorBoard, copy your Studio URL and replace `lab?` with `proxy/6006/` as follows\. 

   ```
   https://<YOUR_URL>.studio.region.sagemaker.aws/jupyter/default/proxy/6006/
   ```

1. Navigate to the URL to examine your results\. 