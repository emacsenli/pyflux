Gaussian State Space models
==================================

Example
----------

.. code-block:: python
   :linenos:

   import numpy as np
   import pyflux as pf
   import pandas as pd

   nile = pd.read_csv('https://vincentarelbundock.github.io/Rdatasets/csv/datasets/Nile.csv')
   nile.index = pd.to_datetime(nile['time'].values,format='%Y')

   model = pf.LLEV(data=niles,target='Poisson') # local level

   USgrowth = pd.DataFrame(np.log(growthdata['VALUE']))
   USgrowth.index = pd.to_datetime(growthdata['DATE'])
   USgrowth.columns = ['Logged US Real GDP']

   model2 = pf.LLT(data=USgrowth) # local linear trend model

Class Arguments
----------

The **LLEV()** and **LLT()** model class has the following arguments:

* *data* : requires a pd.DataFrame object or an np.array
* *target* : (default: None) specify the pandas column name or numpy index if the input is a matrix. If None, the first column will be chosen as the data.

Class Attributes
----------

The **LLEV()** and **LLT()** model objects hold the following attributes:

Model Attributes:

* *param_no* : number of model parameters
* *data* : the dependent variable held as a np.array
* *data_name* : string variable containing name of the time series
* *data_type* : whether original datatype is numpy or pandas

Parameter Attributes:

The attribute *param.desc* is a dictionary holding information about individual parameters:

* *name* : name of the parameter
* *index* : index of the parameter (begins with 0)
* *prior* : the prior specification for the parameter
* *q* : the variational distribution approximation

Inference Attributes:

* *params* : holds any estimated parameters
* *ses* : holds any estimated standard errors for parameters (MLE/MAP)
* *ihessian* : holds any estimated inverse Hessian (MLE/MAP)
* *chains* : holds trace information for MCMC runs
* *supported_methods* : which inference methods are supported 
* *default_method* : default inference method

Class Methods
----------

**adjust_prior(index,prior)**

Adjusts a prior with the given parameter index. Arguments are:

* *index* : taking a value in range(0,no of parameters)
* *prior* : one of the prior objects listed in the Bayesian Inference section

.. code-block:: python
   :linenos:

   model.list_priors()
   model.adjust_prior(2,ifr.Normal(0,1))

**fit(method)**

Fits parameters for the model. Arguments are:

* *method* : one of ['BBVI',MLE','MAP','M-H','Laplace']
* *printed* : (default: True) whether to print output
* *nsims* : (default: 100000) how many simulations if M-H is chosen
* *cov_matrix* (default: None) covariance matrix for M-H
* *iterations* : (default: 30000) how many iterations if BBVI is chosen
* *step* : (default: 0.001) step size for BBVI

.. code-block:: python
   :linenos:

   model.fit("M-H",nsims=20000)

**list_priors()**

Lists the current prior specification.

**plot_fit()**

Graphs the fit of the model. Optional arguments are:

* *series_type* : 'Filtered' or 'Smoothed'

**plot_predict(h)**

Predicts h timesteps ahead and plots results. Arguments are:

* *h* : (default: 5) how many timesteps to predict ahead
* *past_values* : (default: 20) how many past observations to plot
* *intervals* : (default: True) whether to plot prediction intervals

**plot_predict_is(h)**

Predicts rolling in-sample prediction for h past timestamps and plots results. Arguments are:

* *h* : (default: 5) how many timesteps to predict
* *past_values* : (default: 20) how many past observations to plot
* *intervals* : (default: True) whether to plot prediction intervals

**predict(h)**

Predicts h timesteps ahead and outputs pd.DataFrame. Arguments are:

* *h* : (default: 5) how many timesteps to predict ahead

**predict_is(h)**

Predicts h timesteps ahead and outputs pd.DataFrame. Arguments are:

* *h* : (default: 5) how many timesteps to predict ahead

**simulation_smoother(data, beta)**

Outputs a simulated state trajectory from a simulation smoother. Arguments are:

* *data* : the data to simulate from - use self.data usually.
* *beta* : the parameters to use - use self.params (after fitting a model) usually.


.. code-block:: python
   :linenos:

   model.plot_predict(h=12,past_values=36)