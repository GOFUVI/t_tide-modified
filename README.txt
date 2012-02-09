This modification was made in order to be able to apply harmonic analysis to VESSEL MOUNTED ACOUSTIC DOPPLER CURRENT PROFILER (VMADCP) data, which do not have regular sampling intervals

- This code is not made in order to choose which are the significant tidal components in an study area

- We must, previously know what are the significant tidal components 

- VMADCP data must have enough temporal resolution to resolve the tidal components.  The duration of the cruises (therefore, temporal resolution of VMADCP data) will depend on which tidal components we want to resolve. For example to resolve the M2 component, we must have at least 12 hours of data (M2 frequency)

- Previous to apply harmonic analysis, we will need to calculate the observation times which usually are not regularly spaced. We will create a time vector with these observation times (in hours): tin
EX.- tin=[0,0.5,3,5,8,12,21,34]'

- We decided to put as an output the explained variability (var). This variable was already defined in the regular t_tide code
 
T_TIDE_MODIF
function [nameu,fu,tidecon,xout,VAR]=t_tide(xin,TIN,varargin);


T_TIDE
function [nameu,fu,tidecon,xout]=t_tide(xin,varargin);




THE BASIC EQUATION THAT HARMONIC ANALYSIS RESOLVES IN T_TIDE  (once we have chosen what is the most significant tidal component) IS:
   ---------------
  |  xout=tc*coef |
   --------------
Where,

-  coef=tc\xin; (\ is the matlab operator which solve linear systems of equations)
-  tc=[ones(length(t),1) cos((2*pi)*t*fu') sin((2*pi)*t*fu') ];
-  fu, is the tidal component frequency

 t is defined as follows in the t_tide code

------>>>>>   t=dt*([1:nobs]'-ceil(nobsu/2));  % Time vector for entire time series, centered at series midpoint <<<<<<<------   

dt is defined= 1 as default and we can choose another value, but it must be always a constant value

Instead of t, in t_tide_modif, we will use tin, that is a time vector with the observation times but no necessary with a constant interval.
We will need a new input in the t_tide_modif code:

           
------>>>>>          new INPUT: tin (in hours)

Therefore, in the new code:

 tc=[ones(length(tin),1) cos((2*pi)*tin*fu') sin((2*pi)*tin*fu') ];


Moreover, the central time in the regular t_tide is defined based, again in dt and in the number of observations (knobs):


------>>>>>   centraltime=stime+floor(nobsu./2)./24.0*dt;
 
However, following Pawlowicz,2002:
% The Greenwich phase is the phase referenced to the phase of equilibrium response at 0¼ longitude
%This can be interpreted as reporting the phase of the response at the time when the equilibrium forcing is at its largest positive value at 0¼ longitude.
%It is simplest to find the fitted phase at the central time of the record (t=0)

So, we will assume, in t_tide_modif the centraltime as the startime (time):


------>>>>>     centraltime=stime;


                                                                           This modification of the t_tide code was made by E. Aguiar & J.L. Herrera, 2010
