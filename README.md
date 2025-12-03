# EXP 3 A: IIR-BUTTERWORTH-FITER-DESIGN

## AIM: 

 To design an IIR Butterworth filter  using SCILAB. 

## APPARATUS REQUIRED: 
PC installed with SCILAB. 

## PROGRAM (LPF): 
```c
clc ;  
close ;  
wp=input('Enter the pass band frequency (Radians )= ' );  
ws=input('Enter the stop band frequency (Radians )= ' );  
alphap=input( ' Enter the pass band attenuation (dB)=' );  
alphas=input( ' Enter the stop band attenuation(dB)=' );  
T=input('Enter the Value of sampling Time=');  
omegap=(2/T)*tan(wp/2);  
disp('omegap=',omegap);  
omegas=(2/T)*tan(ws/2);  
disp('omegas=',omegas);    
N=log10(((10^(0.1*alphas))-1)/((10^(0.1*alphap))-1))/(2*log10(omegas/omegap));  
disp('N=',N); 
N=ceil(N);  
disp('Round off value of N=',N);  
omegac=omegap/(((10^(0.1*alphap)) -1)^(1/(2* N)));  
disp('omegac=',omegac);  
disp('Normalised Analog LPF Transfer function H(S)=');  
hs_Normalised = analpf(N,'butt',[0,0],1);  
disp(hs_Normalised);  
disp('Analog LPF Transfer function H(S)=');  
hs= analpf(N,'butt',[0,0],omegac);  
disp(hs);  
z=poly(0,'z'); 
Hz=horner(hs,(2/ T)*((z -1)/(z+1))) 
disp('Digital LPF Transfer function H(Z)=');  
disp(Hz);  
HW=frmag(Hz,512);  
w=0:%pi/511:%pi ;  
plot(w/%pi,abs(HW));  
xlabel(' Normalized Digital Frequency w');  
ylabel('Magnitude '); 
title(' Frequency Response of Butterworth IIR LPF');

//CONSOLE WINDOW:
//Enter the pass band frequency (Radians )= 0.2*%pi
//Enter the stop band frequency (Radians )= 0.6*%pi 
//Enter the pass band attenuation (dB)=2
//Enter the stop band attenuation(dB)=14
//Enter the Value of sampling Time=1
```

## PROGRAM (HPF): 
```c
// Clear Environment
clear;
clc;
close;

// Filter Specifications (example values)
wp = 0.5*%pi;      // Passband frequency in radians
ws = 0.8*%pi;      // Stopband frequency in radians
alphap = 4;      // Passband attenuation in dB
alphas = 18;     // Stopband attenuation in dB
T = 1;           // Sampling time

// Pre-warping analog frequencies
omegap = (2/T) * tan(wp/2);
omegas = (2/T) * tan(ws/2);

// Calculate filter order
N = log10( ((10^(0.1*alphas))-1) / ((10^(0.1*alphap))-1) ) / (2*log10(omegas/omegap));
N = ceil(N);

// Calculate cutoff frequency
omegac = omegap / ((10^(0.1*alphap)-1)^(1/(2*N)));

// Design analog low pass Butterworth prototype
hs = analpf(N, 'butt', [0, 0], omegac);

// Analog Highpass frequency transformation s -> omegac / s
s = poly(0, 's');
h_analog_hpf = horner(hs, omegac ./ s);

// Convert the analog HPF to digital HPF using bilinear transformation
z = poly(0, 'z');
Hz = horner(h_analog_hpf, (2/T)*((z-1)./(z+1)));

// Plot frequency response
Hw = frmag(Hz, 512);
w = 0:%pi/511:%pi;
plot(w/%pi, abs(Hw));
xlabel('Normalized Digital Frequency (×π rad/sample)');
ylabel('Magnitude');
title('Butterworth Highpass IIR Filter Frequency Response');
```


## OUTPUT (LPF) : 

<img width="799" height="400" alt="3l" src="https://github.com/user-attachments/assets/c347a853-8662-4fa0-bd76-b6b7bd02ec94" />


## OUTPUT (HPF) : 

<img width="787" height="399" alt="3h" src="https://github.com/user-attachments/assets/f04d069f-cf90-4928-9999-ecd6b44dee95" />


## RESULT: 

Thus, design of IIR Butterworth filter(LPF & HPF) using SCILAB was successfully completed.
