# SR-GAN
A generator model that will super resolve the low resoluted image. Here i trained SRRESNET model first with mse optimization and then i followed GAN approach for further optimization using this trained SSRESNET with perceptual loss optimization.

<p> Here i developed the model, based on the official paper : <a href="https://arxiv.org/abs/1609.04802">Photo-Realistic Single Image Super-Resolution Using a Generative Adversarial Network </a> Kindly check out this paper !

<h2> Implementation </h2>

<ul>
	<li> Here i used two models, one is Resnet model as generator followed by subpixel block and discriminator model comprised of 7 convolution layers followed by batch norm and leaky relu as activation </li>
	
<h3> Step 1 : Data Preparation </h3>

<ul>
	<li> Here i only used 10k COCO dataset  as of resource constraints<a href="http://images.cocodataset.org/zips/train2014.zip">Link </a> , if you want you can train for the whole COCO dataset.
	</li>
	<li> After downlading the data set , form the tensorflow dataset </li>
	<li> Train dataset - 80 % of total data </li>
	<li> Validation  dataset - 20% of the total data </li>
	<li> for the Test data set i Used the benchmark Set5,Set15,BSDS100 dataset <a href="https://github.com/XPixelGroup/BasicSR/blob/master/docs/DatasetPreparation.md#common-image-sr-datasets"> link </li>
	<li> How to deal with the data what is the folder structure ? all things are clearly mentioned inside the notebook </li>
	<li> After forming the Dataset, for the training and the validation images , Preprocess the images by doing random crop and downscale by image by factor of 4 <li>
	<li> This preprocessing will take a hr image and return a downscaled image lr </li>
	<li> for the Test dataset , we need to find the max possible dimension divisible by 4 , so that we can downsample the image </li>
	<li> Here the images are normalized to have the values between (0,1) </li>
</ul>

<h3> Step 2 : Model Architecture and Training </h3>

<ul>
	<li><h5> SRRESNET </h5>
	<li> In the SRRESNET model ,it contains three block , convolution  block , subpixel block and residual block </li>
	<li> Low resoluted image is passed to the convolution layer and then to the  resnet contains 16 residual block following Batch norm and Leakey relu in each  residual block </li>
	<li> The result from the resnet is passed to the convolution layer and then to the 2 subpixel block for upscale the image by a factor of four where each will upscale by a factor of 2 .</li>
	<li>Finally the result is passed to a convolution layers with sigmoid activation </li>
	<li> Initially this SRRESNET is trained for 10 million steps until convergence and it is optimized with MSE </li>
	<li><h5> GAN </h5>
	<li> It consists of Generator and the Discriminator network </li>
	<li> For the Generator is used the same SRRESNET which is trained before , and optimised with VGG perceptual loss </li>
	<li> the output from the Generator  and the High resolution images is passed to the VGG network and MSE is calculated on the feature insead of pixel outputs which is more perceptually aligned with human perception </li>
	<li> And in addition  to this loss , the classification loss for the predicition of the sr by the discriminator is also considered for the generator training </li>
	<li> Discriminator model contains 7 convolution layers and dense layer to form a binary classifier</li>
	<li> Discriminator is trained with binary classifiction  loss calculated from the high resolution and the super resolved image </li>
	<li> High resolution is passed to the disc model , the we will figure what is the probability of the hr image being real</li>
	<li> for SR image , what is the probability of the sr beiing fake ? </li>
	<li> Both generator and discriminator is trained alternatively</li>
</ul>

<h3> Step 3 : Metrics and results </h3>

<ul>
	<li> Here loss ,PSNR and SSIM is used as metric to evaluate the peformance of the model during training and the validation </li>
	<li> On the Test dataset , the generator produced 0.61 SSIM and 23.877 PSNR </li>
	<li> I also tested some low quality images, it produced amazing results by reconstructing and generting super quality high resolted images</li>
	
</ul>


<h3> References </h3>

<ul>
	<li> I took reference from this pytorch implementation of the SRGAN where the author clearly explained, Kindly check out this <a href="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Super-Resolution">Link</a>
	</li>
</ul>

<h5> I am currently working on building SOTA algorithm on SUPER RESOLUTION from scratch like this  and will build a webapplication for the usage. Stay Tuned... </h5>


