# ASK & FSK
# Aim
Write a simple Python program for the modulation and demodulation of ASK and FSK.
# Tools required
VS Code
# Program
# ASK
```py
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

fs, fc, br, T = 1000, 50, 10, 1
t = np.linspace(0, T, fs, endpoint=False)

b = np.random.randint(0, 2, br)
m = np.repeat(b, fs // br)
c = np.sin(2 * np.pi * fc * t)
a = m * c

d = lfilter(*butter(5, fc/(fs/2), 'low'), a * c)
r = (d[::fs//br] > 0.25).astype(int)

for i, (s, ttl, clr) in enumerate(zip([m, c, a],
                                      ['Message', 'Carrier', 'ASK Signal'],
                                      ['b', 'g', 'r']), 1):
    plt.subplot(4, 1, i)
    plt.plot(t, s, clr)
    plt.title(ttl)
    plt.grid()

plt.subplot(4, 1, 4)
plt.step(range(len(r)), r, where='mid', color='m')
plt.title('Decoded Bits')
plt.grid()

plt.tight_layout()
plt.show()

```
# FSK
```py
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

fs,f1,f2,br,T=1000,30,70,10,1
t=np.linspace(0,T,fs,endpoint=False)

bits=np.random.randint(0,2,br)
bd=fs//br
msg=np.repeat(bits,bd)

c1=np.sin(2*np.pi*f1*t)
c2=np.sin(2*np.pi*f2*t)

fsk=np.concatenate([np.sin(2*np.pi*(f2 if b else f1)*t[i*bd:(i+1)*bd]) for i,b in enumerate(bits)])

b,a=butter(5,f2/(0.5*fs),'low')
x1=lfilter(b,a,fsk*c1)
x2=lfilter(b,a,fsk*c2)

dec=np.array([
    1 if np.sum(x2[i*bd:(i+1)*bd]**2) >
         np.sum(x1[i*bd:(i+1)*bd]**2) else 0
    for i in range(br)
])

demod=np.repeat(dec,bd)

signals=[msg,c1,c2,fsk,demod]
titles=['Message Signal','Carrier f1','Carrier f2',
        'FSK Signal','Demodulated Signal']

colors=['blue','green','red','magenta','black']

plt.figure(figsize=(12,12))

for i in range(5):
    plt.subplot(5,1,i+1)
    plt.plot(t,signals[i],color=colors[i])
    plt.title(titles[i])
    plt.grid()

plt.tight_layout()
plt.show()
```
# Output Waveform
# ASK
<img width="989" height="790" alt="image" src="https://github.com/user-attachments/assets/af9dd80f-2ff6-4d58-8d1f-1d20c8557890" />


# FSK
<img width="1189" height="1190" alt="image" src="https://github.com/user-attachments/assets/1d976845-7bde-401b-8a61-9f4f4937a7f9" />


# Results
Thus, the ASK and FSK performed using python in VS Code
