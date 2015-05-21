from scipy import *
from numpy import *
from matplotlib.pyplot import *
from scipy.integrate import odeint

T = list(range(1000))


Tamb = 263.15
Tcpu = 333.15

Q0 = 2490
Lp = 0.2
Kp = 0.17 #watt/metro*kelvin
Ap = 24
Hp = 0.001 #0.5 - 1000 (watt/metro^2*kelvin)


V = 3
hh = 10.45-V+10*V**0.5
Kh = 5
Ah = 0.005
Ac = 0.0004
Lh = 0.15

Rcv = 1/(Hp*Ap)
Rcd = Lp/(Kp*Ap)

Rcdh = Lh/(Kh*Ac)
Rcvh = 1/(hh*Ah)

Tt = []

def func1 (Qq, t):
    Tq = (Qq/8.3144)-36
    dT = Tq-Tamb
    dTh = Tcpu-Tq
    dTdt = (dTh*2/(Rcdh+Rcvh))-(dT/(Rcv+Rcd))
    return dTdt

Y = odeint (func1, Q0, T)

for i in range(len(T)):
    Tt.append((Y[i][0]/8.3144)-273.15-36)



plot(T, Tt)
show()
