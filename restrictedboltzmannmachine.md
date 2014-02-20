---
title: 
layout: default
---


### Restricted Boltzmann machine

To quote Hinton, a [Boltzmann machine](http://www.scholarpedia.org/article/Boltzmann_machine) is "a network of symmetrically connected, neuron-like units that make stochastic decisions about whether to be on or off." A [restricted Boltzmann machine](http://www.scholarpedia.org/article/Boltzmann_machine#Restricted_Boltzmann_machines) "consists of a layer of visible units and a layer of hidden units with no visible-visible or hidden-hidden connections." That is, its nodes must form a [bipartite graph](https://en.wikipedia.org/wiki/Bipartite_graph). [RBMs](.{{ site.baseurl }}/glossary.html#restrictedboltzmannmachine) are useful for [dimensionality reduction](https://en.wikipedia.org/wiki/Dimensionality_reduction), [classification](https://en.wikipedia.org/wiki/Statistical_classification), [collaborative filtering](https://en.wikipedia.org/wiki/Collaborative_filtering), [feature learning](https://en.wikipedia.org/wiki/Feature_learning) and [topic modeling](https://en.wikipedia.org/wiki/Topic_model). Given their relative simplicity, RBMs are the first neural network we'll tackle.




PARAMETERS - Please also see [the single layer network parameters common to all single networks]({{ site.baseurl }}/singlelayernetwork.html)

k - The number of times to run [contrastive divergence]({{ site.baseurl }}/glossary.html#contrastivedivergence). Each time contrastive divergence is run, it is a sample of the markov chain

composing the restricted boltzmann machine. A typical value of 1 is fine.



BIAS - HIDDEN AND VISIBLE

SHOW CONNECTION BETWEEN MATRIX AND DRAWING OF NODES, MAPPING NUMBERS TO CONNECTIONS

EXPLAIN WHAT THE WEIGHTS MEAN

### Initiating a restricted Boltzmann machine 

Setting up a single-thread restricted Boltzmann machine is easy. 

To create the machine, you simply instantiate an object of the [class](../doc/com/ccc/deeplearning/rbm/RBM.html).


		RBM rbm = new RBM.Builder().numberOfVisible(784).numHidden(400).withRandom(rand)
				.useRegularization(false).withMomentum(0).build();


The RBM uses the builder pattern to setup config, for example, this will handle the following:

           Number of visible (input) units: 784

           Number of hidden (output) units: 400 


           withRandom(specify an RNG)


           useRegularization(use L2?)


           Momentum: Use momentum or not?



Next, create a training set for the machine. For the sake of visual brevity, a toy, two-dimensional data set is included in the code below. (With large-scale projects, training sets are clearly much more substantial.)


            double[][] data = new double[][]
				{
				{1,1,1,0,0,0},
				{1,0,1,0,0,0},
				{1,1,1,0,0,0},
				{0,0,1,1,1,0},
				{0,0,1,1,0,0},
				{0,0,1,1,1,0},
				{0,0,1,1,1,0}
			};

		DoubleMatrix d = new DoubleMatrix(data);

Now that you have instantiated the machine and created the training set, it's time to train the network. 
This will run contrastive divergence till convergence with a learning rate of 0.01, a k of 1, and the input
specified earlier.  The last snippet will construct a new training set and show the reconstructed input.

Note that RBMs take binary input.



		rbm.trainTillConvergence(0.01,1,d);
		
        double[][] testData = new double[][]
			{
			    {1, 1, 0, 0, 0, 0},
				{0, 0, 0, 1, 1, 0}
			};

		DoubleMatrix v = new DoubleMatrix(testData);	

       System.out.println(r.reconstruct(v).toString());


You can test your trained network by feeding it unstructured data and checking the output.

A trained RBM will learn the structure of the data fed to it. The only thing to understand here

is that the RBM learns how to reconstruct the data.

You can intrepet the numbers as percentages. Anywhere where the number is not zero in the reconstruct

is a good indication that the network learned the input. We can get in to a better example

in some tutorials.