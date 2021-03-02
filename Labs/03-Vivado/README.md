## 1.Logic table


| **Dec. equivalent** | **B[1:0]** | **A[1:0]** | **B is greater than A** | **B equals A** | **B is less than A** |
| :-: | :-: | :-: | :-: | :-: | :-: |
| 0 | 0 0 | 0 0 | 0 | 1 | 0 |
| 1 | 0 0 | 0 1 | 0 | 0 | 1 |
| 2 | 0 0 | 1 0 | 0 | 0 | 1 |
| 3 | 0 0 | 1 1 | 0 | 0 | 1 |
| 4 | 0 1 | 0 0 | 1 | 0 | 0 |
| 5 | 0 1 | 0 1 | 0 | 1 | 0 |
| 6 | 0 1 | 1 0 | 0 | 0 | 1 |
| 7 | 0 1 | 1 1 | 0 | 0 | 1 |
| 8 | 1 0 | 0 0 | 1 | 0 | 0 |
| 9 | 1 0 | 0 1 | 1 | 0 | 0 |
| 10 | 1 0 | 1 0 | 0 | 1 | 0 |
| 11 | 1 0 | 1 1 | 0 | 0 | 1 |
| 12 | 1 1 | 0 0 | 1 | 0 | 0 |
| 13 | 1 1 | 0 1 | 1 | 0 | 0 |
| 14 | 1 1 | 1 0 | 1 | 0 | 0 |
| 15 | 1 1 | 1 1 | 0 | 1 | 0 |

## Code

**mux_2bit_4to1.vhdl**

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;


entity mux_2bit_4to1 is
    port (
          a_i   : in std_logic_vector(2 - 1 downto 0);
          b_i   : in std_logic_vector(2 - 1 downto 0);
          c_i   : in std_logic_vector(2 - 1 downto 0);
          d_i   : in std_logic_vector(2 - 1 downto 0);
          sel_i : in std_logic_vector(2 - 1 downto 0);
          
          f_o   : out std_logic_vector(2 - 1 downto 0)
         );
end entity mux_2bit_4to1;

architecture Behavioral of mux_2bit_4to1 is
begin

    f_o <= a_i when (sel_i = "00") else
           b_i when (sel_i = "01") else
           c_i when (sel_i = "10") else
           d_i;

end Behavioral;
```

**tb_mux_2bit_4to1.vhdl**

```vhdl
library ieee;
use ieee.std_logic_1164.all;


entity tb_mux_2bit_4to1 is

end entity tb_mux_2bit_4to1;

architecture testbench of tb_mux_2bit_4to1 is

    signal s_a       : std_logic_vector(2 - 1 downto 0);
    signal s_b       : std_logic_vector(2 - 1 downto 0);
    signal s_c       : std_logic_vector(2 - 1 downto 0);
    signal s_d       : std_logic_vector(2 - 1 downto 0);
    signal s_sel       : std_logic_vector(2 - 1 downto 0);
    
    signal s_f       : std_logic_vector(2 - 1 downto 0);

begin

    uut_mux_2bit_4to1 : entity work.mux_2bit_4to1
        port map(
            a_i           => s_a,
            b_i           => s_b,
            c_i           => s_c,
            d_i           => s_d,
            sel_i           => s_sel,
            f_o           => s_f
        );


    p_stimulus : process
    begin
        -- Report a note at the begining of stimulus process
        report "Stimulus process started" severity note;

        s_d <= "00"; s_c <= "01"; s_b <= "01"; s_a <= "01"; s_sel <= "00"; wait for 50 ns;
        
        s_a <= "00"; wait for 50 ns;
        s_b <= "11"; wait for 50 ns;
        
        s_sel <= "01"; wait for 50 ns;
        s_c <= "01"; wait for 50 ns;
        s_b <= "10"; wait for 50 ns;  
        
        s_d <= "11";  s_c <= "11"; s_b <= "01"; s_a <= "00"; 
        s_sel <= "10"; wait for 50 ns;  
        
        s_d <= "01";  s_c <= "00"; s_b <= "00"; s_a <= "01"; 
        s_sel <= "11"; wait for 50 ns;  
        
        s_d <= "11";  s_c <= "11"; s_b <= "01"; s_a <= "00"; 
        s_sel <= "10"; wait for 50 ns; 
               
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;

end architecture testbench;
```



## EPWare

![EPWare](images/EPWare.PNG)

## EDAplayground link

[https://www.edaplayground.com/x/qvYn](https://www.edaplayground.com/x/qvYn)


## Vivado tutorial

**New project**
1. Create new project (Quick start -> Create Project)
![EPWare](images/1.PNG)
2. Click "Next"
![EPWare](images/2.PNG)
3. Name your project and select folder to save your project in
![EPWare](images/3.PNG)
4. Check "RTL Project"
![EPWare](images/4.PNG)
5. Create Source file
![EPWare](images/5.PNG)
6. Click "Next"
![EPWare](images/6.PNG)
7. Select "Boards" in the upper part of the window. Then select your board
![EPWare](images/7.PNG)
8. Click "Finish"
![EPWare](images/8.PNG)
9. Click "Ok"
![EPWare](images/9.PNG)

## Add testbench file
1. File -> Add Sources...
![EPWare](images/10.PNG)
2. Check "Add or create simulation sources"
![EPWare](images/11.PNG)
3. Select your File type and name
![EPWare](images/12.PNG)
4. Done
![EPWare](images/13.PNG)

