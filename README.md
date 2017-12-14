# pyquist
Python module for calculating intrinsic or observed frequencies or amplitudes relative to the Nyquist frequency.

I wrote these functions for the analysis in the paper Bell et al. 2017, [*Destroying Aliases from the Ground and Space: Super-Nyquist ZZ Cetis in K2 Long Cadence Data*](http://adsabs.harvard.edu/abs/2017ApJ...851...24B), ApJ, 851, 24.

The included functions make the following computations:
 - subfreq: observed sub-Nyquist frequency given intrinsic frequency
 - superfreq: candidate intrinsic frequencies given observed sub-Nyquist frequency
 - subamps: observed amplitude phase smearing factor given intrinsic frequency relative to Nyquist (assuming no observational overhead)
 - superamps: intrinsic amplitude correction factor given intrinsic frequency relative to Nyquist (assuming no observational overhead)
 
 The subfreqs and subamp functions are great for observation planning, while I find the superfreq and superamp functions more useful for interpreting observations.
 
 As an example, this reproduction of Figure 1 from [Bell et al. (2017)](http://adsabs.harvard.edu/abs/2017ApJ...851...24B) is easily made with the code that follows:
 ![Reproduced Figure 1 from Bell et al. 2017](https://github.com/keatonb/pyquist/blob/master/pyquist_demo.png)
 
 ```
#Sample observed intrinsic frequencies
freqsample = np.linspace(0,10,101)

#What observed (sub-Nyquist) frequencies and amplitudes do these exhibit?
obsfreqs = pq.subfreq(freqsample)
obsamps = pq.subamp(freqsample)

#Set up the figure
fig, ax1 = plt.subplots(figsize=(6,4))
c2='#386cb0' #pretty blue
#Second axes for second curve
ax2 = ax1.twinx()

#Plot frequencies
ax1.plot(freqsample,obsfreqs,c=c2)

#Plot amplitudes
ax2.plot(freqsample,obsamps,c='black')

#Everything below is just modifying the axes
ax1.set_xlabel(r'$f_{\rm intrinsic}/f_{\rm Nyquist}$',size=16)
ax1.set_ylabel(r'$A_{\rm measured}/A_{\rm intrinsic}$',size=16)
ax1.set_xticks(range(11))
ax2.tick_params('x', colors='black')
ax2.set_ylabel(r'$f_{\rm measured}/f_{\rm Nyquist}$', color=c2,size=16)
ax2.tick_params('y', colors=c2)
ax2.yaxis.labelpad = -1
ax2.set_yticklabels(['0','','','','','1'])
ax1.set_xlim(0,10)
ax2.set_xlim(0,10)
ax1.set_ylim(0,1)
ax2.set_ylim(0,1)

plt.tight_layout()
plt.show()
 ```
