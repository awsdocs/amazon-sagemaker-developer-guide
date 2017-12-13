# Step 3\.2\.2: Explore the Training Dataset<a name="ex1-preprocess-data-inspect"></a>

Typically, you explore training data to determine what you need to clean up and which transformations to apply to improve model training\. For this exercise, you don't need to clean up the MNIST dataset\. Simply display one of the images in the `train_set` dataset\.

```
%matplotlib inline
import matplotlib.pyplot as plt
plt.rcParams["figure.figsize"] = (2,10)


def show_digit(img, caption='', subplot=None):
    if subplot==None:
        _,(subplot)=plt.subplots(1,1)
    imgr=img.reshape((28,28))
    subplot.axis('off')
    subplot.imshow(imgr, cmap='gray')
    plt.title(caption)

show_digit(train_set[0][30], 'This is a {}'.format(train_set[1][30]))
```

`train_set` contains the following data:

+ `train_set[0]` contains images\. 

+ `train_set[1]` contains labels\. 

The code uses the `matplotlib` library to get and display the 31th image from the training dataset\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/ironman-30th-image-trainset.png)

**Next Step**  
[Step 3\.2\.3: Transform the Training Dataset and Upload It to S3](ex1-preprocess-data-transform.md)