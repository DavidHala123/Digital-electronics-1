## 1.Preparation


![Characteristic equations](Images/eq_flip_flops.png)
<!--
\begin{align*}
    q_{n+1}^D =&~ \\
    q_{n+1}^{JK} =&\\
    q_{n+1}^T =&\\
\end{align*}-->

   | **clk** | **d** | **q(n)** | **q(n+1)** | **Comments** |
   | :-: | :-: | :-: | :-: | :-- |
   | ![rising](Images/eq_uparrow.png) | 0 | 0 | 0 | Sampled and stored |
   | ![rising](Images/eq_uparrow.png) | 0 | 1 | 0 | Sampled and stored |
   | ![rising](Images/eq_uparrow.png) | 1 | 0 | 1 | Sampled and stored |
   | ![rising](Images/eq_uparrow.png) | 1 | 1 | 1 | Sampled and stored |

   | **clk** | **j** | **k** | **q(n)** | **q(n+1)** | **Comments** |
   | :-: | :-: | :-: | :-: | :-: | :-- |
   | ![rising](Images/eq_uparrow.png) | 0 | 0 | 0 | 0 | No change |
   | ![rising](Images/eq_uparrow.png) | 0 | 0 | 1 | 1 | No change |
   | ![rising](Images/eq_uparrow.png) | 0 | 0 | 1 | 1 | Reset |
   | ![rising](Images/eq_uparrow.png) | 0 | 0 | 1 | 0 | Reset |
   | ![rising](Images/eq_uparrow.png) | 1 | 0 | 0 | 1 | Set |
   | ![rising](Images/eq_uparrow.png) | 1 | 0 | 1 | 1 | Set |
   | ![rising](Images/eq_uparrow.png) | 1 | 1 | 0 | 1 | Toggle |
   | ![rising](Images/eq_uparrow.png) | 1 | 1 | 1 | 0 | Toggle |

   | **clk** | **t** | **q(n)** | **q(n+1)** | **Comments** |
   | :-: | :-: | :-: | :-: | :-- |
   | ![rising](Images/eq_uparrow.png) | 0 | 0 | 0 | No change |
   | ![rising](Images/eq_uparrow.png) | 0 | 1 | 1 | No change |
   | ![rising](Images/eq_uparrow.png) | 1 | 0 | 1 | Invert (Toggle) |
   | ![rising](Images/eq_uparrow.png) | 1 | 1 | 0 | Invert (Toggle) |


<!--


**f(B<A) = (b1+b1+a1+a1) . (b1+!b0+a1+a1) . (b1+!b0+a1+!a0) . (!b1+b0+a1+a0) . (!b1+b0+a1+!a0) . (!b1+b0+!a1+a0) . (!b1+!b0+a1+a0) . (!b1+!b0+a1+!a0) . (!b1+!b0+!a1+a0) . (!b1+!b0+!a1+!a0)**


## K-maps

![Karnaugh](images/excel_karnaugh3.png)

**B>A:**

SOP --> f(b>a) = (b1.!a1) + (b1.!a0) + (b0.!a1.!a0)

POS --> f(b>a) = (b1+!a1) . (b1+!a0) . (b0+!a1+!a0)



**B=A:**

POS --> f(B=A) = (!b1.!b0.!a1.!a0) + (!b1.b0.!a1.a0) + (b1.b0.a1.a0) + (b1.!b0.a1.!a0)

SOP --> f(B=A) = (!b1+!b0+!a1+!a0) . (!b1+b0+!a1+a0) . (b1+b0+a1+a0) . (b1+!b0+a1+!a0)



**B<A:**

POS --> f(B<A) = (b0.!a1) + (b1.b0) + (b1.!a1) + (!a1.!a0)

SOP --> f(B<A) = (b0+!a0) . (b1+b0) . (b1+!a0) . (!a1+!a0)

## Code

**design.vhdl**

```vhdl
library IEEE;
use IEEE.std_logic_1164.all;


entity comparator_4bit is
    port(
        a_i           : in  std_logic_vector(4 - 1 downto 0);
        b_i           : in  std_logic_vector(4 - 1 downto 0);

        

        B_greater_A_o : out std_logic;
        B_equals_A_o  : out std_logic;
        B_less_A_o    : out std_logic      
    );
end entity comparator_4bit;


architecture Behavioral of comparator_4bit is
begin

    B_greater_A_o  <= '1' when (b_i > a_i) else '0';
    B_equals_A_o   <= '1' when (b_i = a_i) else '0';
    B_less_A_o     <= '1' when (b_i < a_i) else '0';


end architecture Behavioral;

```

**testbench.vhdl**

```vhdl
library IEEE;
use IEEE.std_logic_1164.all;


entity tb_comparator_4bit is

end entity tb_comparator_4bit;


architecture testbench of tb_comparator_4bit is

    signal s_a       : std_logic_vector(4 - 1 downto 0);
    signal s_b       : std_logic_vector(4 - 1 downto 0);
    signal s_B_greater_A : std_logic;
    signal s_B_equals_A  : std_logic;
    signal s_B_less_A    : std_logic;

begin

    uut_comparator_4bit : entity work.comparator_4bit
        port map(
            a_i           => s_a,
            b_i           => s_b,
            B_greater_A_o => s_B_greater_A,
            B_equals_A_o  => s_B_equals_A,
            B_less_A_o    => s_B_less_A
        );

   
    p_stimulus : process
    begin
        
        report "Stimulus process started" severity note;



        s_b <= "0000"; s_a <= "0000"; 
        wait for 100 ns;
        assert ((s_B_greater_A = '0') and (s_B_equals_A = '1') and (s_B_less_A = '0'))
        report "Chyba pro kombinaci: 0000, 0000" severity error;
        
        s_b <= "0100"; s_a <= "0001"; 
        wait for 100 ns;
        assert ((s_B_greater_A = '1') and (s_B_equals_A = '1') and (s_B_less_A = '0'))
        report "Chyba pro vstupní hodnoty: 0100, 0001" severity error;
       
        s_b <= "0100"; s_a <= "0010"; 
        wait for 100 ns;
        assert ((s_B_greater_A = '1') and (s_B_equals_A = '0') and (s_B_less_A = '0'))
        report "Chyba pro vstupní hodnoty: 0100, 0010" severity error;
        
        s_b <= "0100"; s_a <= "0011"; 
        wait for 100 ns;
        assert ((s_B_greater_A = '1') and (s_B_equals_A = '0') and (s_B_less_A = '1'))
        report "Chyba pro vstupní hodnoty: 0100, 0011" severity error;

		s_b <= "0101"; s_a <= "0100"; 
        wait for 100 ns;
        assert ((s_B_greater_A = '0') and (s_B_equals_A = '0') and (s_B_less_A = '0'))
        report "Chyba pro vstupní hodnoty: 0101, 0100" severity error;
        
        s_b <= "0101"; s_a <= "0101"; 
        wait for 100 ns;
        assert ((s_B_greater_A = '0') and (s_B_equals_A = '1') and (s_B_less_A = '0'))
        report "Chyba pro vstupní hodnoty: 0101, 0101" severity error;
        
        s_b <= "0101"; s_a <= "0110"; 
        wait for 100 ns;
        assert ((s_B_greater_A = '0') and (s_B_equals_A = '0') and (s_B_less_A = '1'))
        report "Chyba pro vstupní hodnoty: 0101, 0110" severity error;
        
        s_b <= "0101"; s_a <= "0111"; 
        wait for 100 ns;
        assert ((s_B_greater_A = '0') and (s_B_equals_A = '0') and (s_B_less_A = '1'))
        report "Chyba pro vstupní hodnoty: 0101, 0111" severity error;
        
        s_b <= "1110"; s_a <= "0100"; 
        wait for 100 ns;
        assert ((s_B_greater_A = '1') and (s_B_equals_A = '0') and (s_B_less_A = '0'))
        report "Chyba pro vstupní hodnoty: 1110, 0100" severity error;
        
        s_b <= "1110"; s_a <= "0101"; 
        wait for 100 ns;
        assert ((s_B_greater_A = '1') and (s_B_equals_A = '0') and (s_B_less_A = '0'))
        report "Chyba pro vstupní hodnoty: 1110, 0101" severity error;
        
        s_b <= "1110"; s_a <= "0110"; 
        wait for 100 ns;
        assert ((s_B_greater_A = '1') and (s_B_equals_A = '0') and (s_B_less_A = '0'))
        report "Chyba pro vstupní hodnoty: 1110, 0110" severity error;
        
         s_b <= "1110"; s_a <= "0111"; 
        wait for 100 ns;
        assert ((s_B_greater_A = '1') and (s_B_equals_A = '0') and (s_B_less_A = '0'))
        report "Chyba pro vstupní hodnoty: 1110, 0111" severity error;
        
        s_b <= "1111"; s_a <= "0100"; 
        wait for 100 ns;
        assert ((s_B_greater_A = '1') and (s_B_equals_A = '0') and (s_B_less_A = '0'))
        report "Chyba pro vstupní hodnoty: 1111, 0100" severity error;
        
        s_b <= "1111"; s_a <= "1101"; 
        wait for 100 ns;
        assert ((s_B_greater_A = '1') and (s_B_equals_A = '0') and (s_B_less_A = '0'))
        report "Chyba pro vstupní hodnoty: 1111, 1101" severity error;
        
        s_b <= "1111"; s_a <= "1110"; 
        wait for 100 ns;
        assert ((s_B_greater_A = '1') and (s_B_equals_A = '0') and (s_B_less_A = '0'))
        report "Chyba pro vstupní hodnoty: 1111, 1110" severity error;
        
        s_b <= "1111"; s_a <= "1111"; 
        wait for 100 ns;
        assert ((s_B_greater_A = '0') and (s_B_equals_A = '1') and (s_B_less_A = '0'))
        report "Chyba pro vstupní hodnoty: 1111, 1111" severity error;


        
        
       
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;

end architecture testbench;
```


## Log
```
analyze design.vhd
analyze testbench.vhd
elaborate tb_comparator_4bit
testbench.vhd:33:9:@0ms:(report note): Stimulus process started
testbench.vhd:44:9:@200ns:(assertion error): Chyba pro vstupní hodnoty: 0100, 0001
testbench.vhd:54:9:@400ns:(assertion error): Chyba pro vstupní hodnoty: 0100, 0011
testbench.vhd:59:9:@500ns:(assertion error): Chyba pro vstupní hodnoty: 0101, 0100
testbench.vhd:121:9:@1600ns:(report note): Stimulus process finished
Finding VCD file...
./dump.vcd
[2021-02-23 05:49:04 EST] Opening EPWave...
Done

```

## EPWare

![EPWare](images/EPWare.PNG)

## EDAplayground link

[https://www.edaplayground.com/x/qvYn](https://www.edaplayground.com/x/qvYn)


