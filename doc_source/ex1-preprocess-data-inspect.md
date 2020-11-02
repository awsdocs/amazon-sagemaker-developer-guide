# Step 4\.2: Explore the Training Dataset<a name="ex1-preprocess-data-inspect"></a>

Typically, you explore training data to determine what you need to clean up and which transformations to apply to improve model training\. For this exercise, you don't need to clean up the MNIST dataset\. 

**To explore the dataset**
+ Type the following code in a cell in your notebook and run the cell to display the first 10 images in `train_set`:

  ```
  %matplotlib inline
  import matplotlib.pyplot as plt
  
  plt.rcParams["figure.figsize"] = (2,10)
  
  for i in range(0, 10):
      img = train_set[0][i]
      label = train_set[1][i]
      img_reshape = img.reshape((28,28))
      imgplot = plt.imshow(img_reshape, cmap='gray')
      print('This is a {}'.format(label))
      plt.show()
  ```

  `train_set` contains the following structures:
  + `train_set[0]` – Contains images\. 
  + `train_set[1]` – Contains labels\. 

  The code uses the `matplotlib` library to get and display the first 10 images from the training dataset\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sagemaker-10-images.png)

**Next Step**  
[Step 4\.3: Transform the Training Dataset and Upload It to Amazon S3](ex1-preprocess-data-transform.md)