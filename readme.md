# PINN for model parameter estimation and response prediction of SDoF system 

<h2>Objective</h2>
<ul>
<li>To estimate damping coefficient of sdof system.</li>
<li>To predict the earthquake response of the system.</li>
</ul>
<br></br>


<h2>Use Cases</h2>
<ul>
<li>Can be used for  design of vibration control strategies</li>
<li>Can be used for predicting future earthquake responses of structures.</li>
</ul>
<br></br>



<center><h2>Overview</h2></center>
<br></br>
<p>

        Physics-informed neural networks are gaining widespread attention among the scientific and engineering community. The basic concept, especially with physics-informed neural networks, is to use physics laws in the form of differential equations to train neural networks. This differs significantly from utilising neural networks as surrogate models trained with data acquired at a 26 combination of input and output values. Thus Physics-informed neural networks can be used to solve the forward problem (estimation of response) and the inverse problem (model parameter identification).
 </p>
<br>
<center><img src="images\Overview.jpg"></center>
<br></br>


<center><h2>Seismic excitation of SDOF linear system</h2></center>
<br></br>

        In this study, the motion of a linear SDOF is considered. The equation of motion of the single degree of freedom system, shown below for the support acceleration defined by ÿg can be written as: Mÿ + Cẏ +Ky = -Mÿg ,or alternatively, ÿ= M-1 (- Mÿg- Cẏ - Ky) (1) The study assumes that while the masses and spring coefficients are known, the damping coefficient is unknown. Initially, the available data, which contains the ground acceleration as input data and corresponding displacement response as output data will be given to the physics-informed neural network. The neural network is modeled in such a way that the damping coefficient is the learning parameter. Later during training, the value of this damping coefficient gets updated. Once the coefficient is estimated, the equations of motion can be used to predict the mass displacements given any input conditions. This can be useful for the design of vibration control strategies.The whole process is illustrated in Fig below.

<center><img src="images\Sesls.jpg"></center>


<center><h2>Data Generation</h2></center>
<br></br>
<ul>
<li>Obtained training and testing data using newmark’s method</li>
<ul>
<li>g=386 in/s2</li>                      
<li>M=(200/g)lb</li>               
<li>K=20.4 kip/in</li> 
<li>C=0.13lbfs/in</li>
</ul>
</ul>


<center><h2>Model Architecture</h2></center>
<br></br>
<p>

        The model used in the study is Hybrid physics-informed Recurrent Neural Network. A custom
        RNN cell is made based on the Runge-Kutta integration. Figure below illustrates the Runge-Kutta
        integration cell which was implemented in the RNN Here, the TensorFlow native recurrent
        neural network class RNN is used to effectively march in time. All the parameters except the
        damping ratio are provided to the model so that at the end of the training the model will learn the
        value of the damping coefficient, C. The coefficient of damping is initialized as 1 before training
        the model. In order to implement specialized recurrent neural networks for numerical integration,
        the only requirements are that computations stay within linear algebra complexity (so that
        computational cost stays comparable to any other neural network architecture) and gradients with
        respect to trainable parameters are made available (so that backpropagation can be used for
        optimization).

</p>
<center><img src="images\architecture.jpg">
<br></br>

<center><h2>Results</h2></center>
<br></br>
<p>

        The trained model will find the optimal coefficient of damping for the given structure of
        consideration. The initial assumption of the coefficient of damping was 1. After training the
        model with training data the value of the coefficient of damping got updated and it became
        0.13039, which is very close to the actual value of the coefficient of damping.
        For the evaluation of the model, the trained model is to be evaluated with the test data.
        Fig. 2.23 is the screenshot of the evaluation process done in colab. The model equipped with the
        optimal coefficient of damping for the structure was able to predict the displacement responses
        from the given ground acceleration data. This model was evaluated on a test using mean absolute
        error which is the sum of the absolute difference between the predicted and the true value. Here
        we have obtained a mean absolute error of 1.0404 for the testing data. Fig. 2.24 shows the test set
        results.
</p>
<center><img src="images\result1.jpg">
<br></br>
