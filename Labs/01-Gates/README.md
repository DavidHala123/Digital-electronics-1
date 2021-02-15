## 1. Git

Odkaz na [Github](https://github.com/DavidHala123/Digital-Electronics-1)

## Rovnice a tabulky
**Rovnice**

![Logic function](images/equations.png)

**Tabulky hodnot**

| **a** | **b** |**c** | **f(a,b,c)** |**f(a,b,c)NAND** |**f(a,b,c)NOR** |
| :-: | :-: | :-: | :-: | :-: | :-: |
| 0 | 0 | 0 | 1 | 1 | 1 |
| 0 | 0 | 1 | 1 | 1 | 1 |
| 0 | 1 | 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 0 | 0 | 0 |
| 1 | 0 | 0 | 0 | 0 | 0 |
| 1 | 0 | 1 | 1 | 1 | 1 |
| 1 | 1 | 0 | 0 | 0 | 0 |
| 1 | 1 | 1 | 0 | 0 | 0 |

```vhdl
architecture dataflow of gates is 
begin 
	 f_i= ((not b_i) or a_i) and ((not c_i) or (not b_i)) 
     fnand_o <= not(not(b_i or a_i) and not(a_i or b_i));
     fnor_o <= not(b_i or not(a_i)) or not(c_i or b_i);
     
end architecture dataflow;
```
