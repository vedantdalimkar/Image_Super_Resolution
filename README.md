# Image Super Resolution

Trained all models on Kaggle.

SRCNN notebook - https://www.kaggle.com/vedantvijaydalimkar/srcnn-comparison/edit/run/88250590

SRResNet notebook - https://www.kaggle.com/vedantvijaydalimkar/srresnet-comparison

SRResNet w/ perceptual loss - https://www.kaggle.com/vedantdalimkar/srresnet-perceptual-loss

## SRCNN
Implemented the model based on the [original super resolution paper](https://arxiv.org/pdf/1501.00092v3.pdf). Architecture of the model-

![image](https://user-images.githubusercontent.com/67591647/154806101-54cbee6f-afd5-40e8-aae7-c55990f4975b.png)

Used Pytorch Lightning's built in learning rate finder to find the optimal learning rate. Used the largest possible batch size that could fit in the memory which was 16. Implemented a custom dataset class, modified the getitem method which applies 2 transformations on a single image and returns 2 images - one high resolution image and one low resolution image. Had to resize all images to a common size as they had varying sizes.

The function compare() compares the performance of the model for different types of image interpolations done while preprocessing (resizing) the images.

Results: Using Bilinear interpolation gives the best results compared to Bicubic/Nearest Neighbour interpolations.

Comparison:

###### High Resolution images
![image](https://user-images.githubusercontent.com/67591647/154810718-aecc7b30-e191-4ba8-9d86-bd53c208058d.png)

###### Model output for Bilinear interpolation
![image](https://user-images.githubusercontent.com/67591647/154810727-6d41e359-cbfb-40d1-9476-00d2ebfe4d2d.png)

###### Model output for Nearest Neighbour interpolation
![image](https://user-images.githubusercontent.com/67591647/154810738-3998ff9c-00bc-41a9-b2ab-a14678e1bbe1.png)

###### Model output for Bicubic interpolation
![image](https://user-images.githubusercontent.com/67591647/154810743-23d42219-3ea0-460c-ad4b-0f7b1d011548.png)

MSE Loss on test set:

Bilinear - 0.002147814491763711

Nearest Neighbour - 0.005143341142684221

Bicubic - 0.002530161291360855


## SRResNet
Followed the [SRGAN paper](https://arxiv.org/pdf/1609.04802v5.pdf) for the implementation. Used learning rate of 1e-4 as mentioned in the paper. In the paper, 'B' is defined as the number of residual blocks used in the network. In the paper, they have used B = 16. Ran the model for different B values (tried B=8,12,16), got the best results for B = 12. Architecture of the model-
![image](https://user-images.githubusercontent.com/67591647/154810442-e66d4fbc-7f3a-4f58-836c-7893520081c1.png)
(k stands for kernel size, n for the number of feature maps and s stands for the stride use)

Also implemented SRResNet with a perceptual loss function where a pretrained CNN model is used as loss network. The objective of the loss function is to minimise the differences in the feature representations which are extracted from the pretrained loss network. Used a VGG 16 pretrained model as the loss network. Perceptual loss can be explained simply as: (quoting from the perceptual loss paper by J Johnson)

>Rather than encouraging the pixels of the
output image to exactly match the pixels of the target image y, we
instead encourage them to have similar feature representations as computed by
the loss network.

Trained SRResNet on some other loss functions too such as L1 loss function and Peak signal-to-noise ratio. (Trained for 10 epochs)

Comparison:
###### High Resolution images
![image](https://user-images.githubusercontent.com/67591647/154813995-a2fb6aec-6705-42db-aae0-59e3055c0aff.png)

###### Model output for SRResNet trained using perceptual loss function
![image](https://user-images.githubusercontent.com/67591647/154813973-55a1df1f-508e-4aa0-b360-4ddaff6339cd.png)

###### Model output for SRResNet trained using Peak signal-to-noise ratio loss function
![image](https://user-images.githubusercontent.com/67591647/154814068-bdfb217b-1f58-4a65-9a55-61e8ed579026.png)

###### Model output for SRResNet trained using MAE loss function
![image](https://user-images.githubusercontent.com/67591647/154816290-59f1b68c-dbad-4958-82db-ded098370029.png)

