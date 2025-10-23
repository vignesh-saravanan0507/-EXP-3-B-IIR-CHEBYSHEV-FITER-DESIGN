# EXP 3 : IIR-CHEBYSHEV-FITER-DESIGN

## AIM: 

 To design an IIR Chebyshev filter  using SCILAB. 

## APPARATUS REQUIRED: 
PC installed with SCILAB. 

## PROGRAM (LPF): 
```
clc ;
close ;
wp=input('Enter the pass band frequency (Radians )= ' );
ws=input('Enter the stop band frequency (Radians )= ' );
alphap=input( ' Enter the pass band attenuation (dB)=' );
alphas=input( ' Enter the stop band attenuation(dB)=' );
T=input('Enter the Value of sampling Time=');
//Pre warping- Bilinear Transformation
omegap=(2/T)*tan(wp/2);
disp(omegap,'omegap=');
omegas=(2/T)*tan(ws/2);
disp(omegas,'omegas=');
//Order of the filter
N=acosh(sqrt(((10^(0.1*alphas))-1)/((10^(0.1*alphap))-1)))/(acosh(omegas/omegap));
disp(N,'N=');
N=ceil(N);
disp(N,'Round off value of N=');
//Cut off frequency
omegac=omegap/(((10^(0.1*alphap)) -1)^(1/(2* N)));
disp(omegac,'omegac=');
Epsilon = sqrt ((10^(0.1*alphap))-1);
disp(Epsilon,'Epsilon=');
[pols ,gn] = zpch1(N, Epsilon,omegap );
disp(gn,'Gain');
disp(pols,'Poles');
hs=poly(gn,'s','coeff')/real(poly(pols,'s'));
disp(hs,'Analog Low pass Chebyshev Filter Transfer function');
z=poly(0,'z');//Defining variable z
Hz=horner(hs,(2/ T)*((z -1)/(z+1)))// Bilinear Transformation
disp(Hz,'Digital LPF Transfer function H(Z)=');
HW=frmag(Hz,512); // Frequency response
w=0:%pi/511:%pi ;
plot(w/%pi,abs(HW));
xlabel(' Normalized Digital Frequency w');
ylabel('Magnitude ');
title(' Frequency Response of Chebyshev IIR LPF');
```
##CONSOLE:
```

Enter the pass band frequency (Radians )= 0.3*%pi

Enter the stop band frequency (Radians )= 0.6*%pi

 Enter the pass band attenuation (dB)=3

 Enter the stop band attenuation(dB)=20

Enter the Value of sampling Time=1


   1.0190509

  "omegap="

   2.7527638

  "omegas="

   1.8116778

  "N="

   2.

  "Round off value of N="

   1.0202615

  "omegac="

   0.9976283

  "Epsilon="

   0.5204667

  "Gain"

  -0.3285928 + 0.7919631i  -0.3285928 - 0.7919631i

  "Poles"

           0.5204667           
   --------------------------  
   0.7351788 +0.6571856s +s^2  

  "Analog Low pass Chebyshev Filter Transfer function"

   0.5204667 +1.0409335z +0.5204667z^2  
   -----------------------------------  
   3.4208077 -6.5296424z +6.0495499z^2  

  "Digital LPF Transfer function H(Z)="
```


## PROGRAM (HPF): 

```
clc;
close;

// ============================
// User Inputs
// ============================
wp = input('Enter the pass band frequency (Radians )= ');  // Passband edge > Stopband edge
ws = input('Enter the stop band frequency (Radians )= ');
alphap = input('Enter the pass band attenuation (dB)= ');
alphas = input('Enter the stop band attenuation (dB)= ');
T = input('Enter the Value of sampling Time= ');

// ============================
// Pre-warping (Bilinear Transformation)
// ============================
omegap = (2/T)*tan(wp/2);   // Passband edge (analog)
disp(omegap,'omegap=');
omegas = (2/T)*tan(ws/2);   // Stopband edge (analog)
disp(omegas,'omegas=');

// ============================
// Order of the HPF
// ============================
// For HPF: use acosh(omegap/omegas)
N = acosh(sqrt(((10^(0.1*alphas))-1)/((10^(0.1*alphap))-1))) / acosh(omegap/omegas);
disp(N,'N=');
N = ceil(N);
disp(N,'Round off value of N=');

// ============================
// Cutoff frequency
// ============================
omegac = omegap / (((10^(0.1*alphap))-1)^(1/(2*N)));
disp(omegac,'omegac=');

// ============================
// Chebyshev Prototype (Normalized LPF)
// ============================
Epsilon = sqrt((10^(0.1*alphap))-1);
disp(Epsilon,'Epsilon=');

[pols, gn] = zpch1(N, Epsilon, 1);   // normalized LPF prototype at 1 rad/s
disp(gn,'Gain');
disp(pols,'Poles');

s = poly(0,'s');   // Laplace variable
hs = poly(gn,'s','coeff') / real(poly(pols,'s'));
disp(hs,'Analog Normalized Chebyshev LPF');

// ============================
// LPF → HPF Transformation (s -> omegac/s)
// ============================
sh = horner(hs, omegac/s);
disp(sh,'Analog Chebyshev High-Pass Filter');

// ============================
// Bilinear Transformation: s -> (2/T)*((z-1)/(z+1))
// ============================
z = poly(0,'z');   // z-domain variable
Hz = horner(sh, (2/T) * ((z-1)/(z+1)));
disp(Hz,'Digital HPF Transfer function H(Z)=');

// ============================
// Frequency Response
// ============================
HW = frmag(Hz, 512);
w = 0:%pi/511:%pi;

plot(w/%pi, abs(HW));
xlabel('Normalized Digital Frequency ×π rad/sample');
ylabel('Magnitude');
title('Frequency Response of Chebyshev IIR High-Pass Filter');
```

##CONSOLE:
```

Enter the pass band frequency (Radians )= 0.6*%pi

Enter the stop band frequency (Radians )= 0.3*%pi

Enter the pass band attenuation (dB)= 3

Enter the stop band attenuation (dB)= 20

Enter the Value of sampling Time= 1


   2.7527638

  "omegap="

   1.0190509

  "omegas="

   1.8116778

  "N="

   2.

  "Round off value of N="

   2.7560340

  "omegac="

   0.9976283

  "Epsilon="

   0.5011886

  "Gain"

  -0.3224498 + 0.7771576i  -0.3224498 - 0.7771576i

  "Poles"

           0.5011886           
   --------------------------  
   0.7079478 +0.6448997s +s^2  

  "Analog Normalized Chebyshev LPF"

              0.5011886s^2              
   -----------------------------------  
   7.5957232 +1.7773653s +0.7079478s^2  

  "Analog Chebyshev High-Pass Filter"

   0.1433786 -0.2867572z +0.1433786z^2  
   -----------------------------------  
       0.4915365 +0.6814259z +z^2       

  "Digital HPF Transfer function H(Z)="
```



## OUTPUT (LPF) : 
<img width="1918" height="1079" alt="image" src="https://github.com/user-attachments/assets/6cc6838c-41e9-4c27-b58e-c43d7319e7a5" />


## OUTPUT (HPF) : 
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/7e0dda30-bbaa-4bbe-9174-9981a8760ac3" />


## RESULT: 
Thus,the IIR Chebyshev filter is verified
