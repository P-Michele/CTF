```
from z3 import *

# Definizione del solver
solver = Solver()

# Dichiarazione delle variabili
param_1 = [Int('param_1[%d]' % i) for i in range(20)]

# Aggiunta delle condizioni
solver.add(
    param_1[19] * -250 + (param_1[19] * param_1[19]) == -15625,
    param_1[18] * -96 + (param_1[18] * param_1[18]) == -2304,
    param_1[17] * -190 + (param_1[17] * param_1[17]) == -9025,
    param_1[16] * -202 + (param_1[16] * param_1[16]) == -10201,
    param_1[15] * -232 + (param_1[15] * param_1[15]) == -13456,
    param_1[14] * -220 + (param_1[14] * param_1[14]) == -12100,
    param_1[13] * -194 + (param_1[13] * param_1[13]) == -9409,
    param_1[12] * -220 + (param_1[12] * param_1[12]) == -12100,
    param_1[11] * -210 + (param_1[11] * param_1[11]) == -11025,
    param_1[10] * -218 + (param_1[10] * param_1[10]) == -11881,
    param_1[9] * -228 + (param_1[9] * param_1[9]) == -12996,
    param_1[8] * -202 + (param_1[8] * param_1[8]) == -10201,
    param_1[7] * -232 + (param_1[7] * param_1[7]) == -13456,
    param_1[6] * -102 + (param_1[6] * param_1[6]) == -2601,
    param_1[5] * -200 + (param_1[5] * param_1[5]) == -10000,
    param_1[4] * -246 + (param_1[4] * param_1[4]) == -15129,
    param_1[3] * -206 + (param_1[3] * param_1[3]) == -10609,
    param_1[2] * -194 + (param_1[2] * param_1[2]) == -9409,
    param_1[1] * -216 + (param_1[1] * param_1[1]) == -11664,
    param_1[0] * -204 + (param_1[0] * param_1[0]) == -10404
)

# Risoluzione delle condizioni
if solver.check() == sat:
    model = solver.model()
    solution = [model[param_1[i]] for i in range(20)]
    print("Soluzione trovata:")
    flag = ' '
    for i in solution:
        flag += i.as_long().to_bytes(1, "little").decode()
    print(flag)
else:
    print("Non è stata trovata alcuna soluzione.")
```
