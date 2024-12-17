## Measly State Diagram

![[Drawing 2024-11-03 23.04.44.excalidraw]]

## Truth table for mux

| Rout(0) | Rout(1) | Rout(2) | Rout(3) | Rout(4) | Rout(5) | Rout(6) | Rout(7) | DINout | Gout | Buswire (Output) |
| ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------ | ---- | ---------------- |
| D       | D       | D       | D       | D       | D       | D       | D       | D      | 1    | G                |
| D       | D       | D       | D       | D       | D       | D       | D       | 1      | 0    | DIN              |
| 1       | D       | D       | D       | D       | D       | D       | D       | 0      | 0    | R0               |
| 0       | 1       | D       | D       | D       | D       | D       | D       | 0      | 0    | R1               |
| 0       | 0       | 1       | D       | D       | D       | D       | D       | 0      | 0    | R2               |
| 0       | 0       | 0       | 1       | D       | D       | D       | D       | 0      | 0    | R3               |
| 0       | 0       | 0       | 0       | 1       | D       | D       | D       | 0      | 0    | R4               |
| 0       | 0       | 0       | 0       | 0       | 1       | D       | D       | 0      | 0    | R5               |
| 0       | 0       | 0       | 0       | 0       | 0       | 1       | D       | 0      | 0    | R6               |
| 0       | 0       | 0       | 0       | 0       | 0       | 0       | 1       | 0      | 0    | R7               |
