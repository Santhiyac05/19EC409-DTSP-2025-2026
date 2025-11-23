# EXP 1 A : COMPUTATION OF DFT USING DIRECT AND FFT
REG NO:212223060247

# AIM: 
To Obtain DFT and FFT of a given sequence in SCILAB.

# APPARATUS REQUIRED: 
PC installed with SCILAB. 

# PROGRAM: 
// DISCRETE FOURIER TRANSFORM 

DIRECT
```
clear; 
xn=[1 2 3 4 5]; 
n1=0:1:length(xn)-1; 
subplot(3,1,1); 
plot2d3(n1,xn); 
xlabel('Time n'); 
ylabel('Amplitude xn'); 
title('Input Sequence'); 
j=sqrt(-1); 
N=length(xn); 
Xk=zeros(1,N); 
for k=0:N-1 
for n=0:N-1 
Xk(k+1)=Xk(k+1)+xn(n+1)*exp((-j*2*%pi*k*n)/N); 
end 
end 
disp(Xk) 
K1=0:1:length(Xk)-1; 
magnitude=abs(Xk) 
subplot(3,1,2); 
plot2d3(K1,magnitude); 
xlabel('frequency(Hz)'); 
ylabel('magnitude(gain)'); 
title('magnitude spectrum'); 
angle = atan(imag(Xk),real(Xk)) 
subplot(3,1,3); 
plot2d3(K1,angle); 
xlabel('frequency(Hz)'); 
ylabel('Phase'); 
title('Phase spectrum');
```
DFT USING FFT
```
clear;  
clc;  
close;  
xn = [1 2 3 4 4 3 2 1]  
n1=0:1:length(xn)-1;  
subplot(2,2,1);  
plot2d3(n1,xn);  
xlabel('Time n');  
ylabel('Amplitude');  
title('Input Sequence');  
10  
Xk = fft(xn);  
K1=0:1:length(Xk)-1;  
magnitude=abs(Xk)  
subplot(2,2,2);  
plot2d3(K1,magnitude);  
xlabel('frequency(Hz)');  
ylabel('magnitude(gain)');  
title('magnitude spectrum');  
angle = atan(imag(Xk),real(Xk))  
subplot(2,2,3);  
plot2d3(K1,angle);  
xlabel('frequency(Hz)');  
ylabel('Phase');  
title('Phase spectrum')  
y= ifft(Xk)  
n2=0:1:length(y)-1;  
subplot(2,2,4)  
plot2d3(n2,y)  
xlabel('Time n');  
ylabel('Amplitude');  
title('Inverse FFT OF X(K)');
```

# OUTPUT: 
DIRECT:
<img width="1904" height="962" alt="image" src="https://github.com/user-attachments/assets/7564ba98-dd93-4bb6-a0c8-7c3f6024884a" />

DFT USING FFT
<img width="1175" height="602" alt="image" src="https://github.com/user-attachments/assets/fd68a5ba-33a5-4b2e-b9a7-23787b8ade97" />


# RESULT: 
Thus, the Discrete Fourier Transform and Fast Fourier Transform of the given sequence were obtained and its magnitude and phase spectrum were plotted.
