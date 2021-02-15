## 1. Git

Link on [Github](https://github.com/DavidHala123/Digital-Electronics-1)

## 2. Demorgans equations
**Equatioions**

![Logic function](images/equations.png)

**Table**

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

**Code**

```vhdl
architecture dataflow of gates is 
begin 
	 f_o <= ((not b_i) and a_i) or ((not c_i) and (not b_i));
     fnand_o <= not ((not ((not b_i) and a_i)) and (not((not c_i) and (not b_i))));
     fnor_o <= not(b_i or (not a_i)) or (not (c_i or b_i));
     
end architecture dataflow;
```
**EPWave**

![EPWave](images/2nd_part.PNG)

[EDA Playground link](https://www.edaplayground.com/x/iMXV)

## 3. Boolean postulates

**Equations**

![Boolean postulates](images/Postulate_Formula.gif)

**Code**

```vhdl
architecture dataflow of gates is 
begin 
architecture dataflow of gates is
begin
    fa_o  <= (x_i and (not x_i));
    fb_o  <= (x_i or (not x_i));
    fc_o  <= (x_i or x_i or x_i);
    fd_o  <= (x_i and x_i and x_i);

end architecture dataflow;
```

**EPWave**

![EPWave](images/3rd_part.PNG)

[EDA Playground link](https://www.edaplayground.com/x/ea5n)

## 4. Distributive laws

**Equations**

![EPWave](images/Distributive_Laws.gif)

**Table**

| **x** | **y** |**z** | **x.y+x.z** |**x.(y+z)** |**(x+y).(x.z)** |**x+(y.z)** |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 | 0 | 1 | 1 |
| 0 | 1 | 0 | 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 1 | 1 | 1 | 1 |
| 1 | 0 | 0 | 0 | 0 | 0 | 0 |
| 1 | 0 | 1 | 1 | 1 | 1 | 1 |
| 1 | 1 | 0 | 0 | 0 | 1 | 1 |
| 1 | 1 | 1 | 1 | 1 | 1 | 1 |

