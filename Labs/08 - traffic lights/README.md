## 1.Preparation

| **Input P** | `0` | `0` | `1` | `1` | `0` | `1` | `0` | `1` | `1` | `1` | `1` | `0` | `0` | `1` | `1` | `1` |
| :-- | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| **State** | A | A | B | C | C | D | A | B | C | D | B | B | B | C | D | B |
| **Output R** | `0` | `0` | `0` | `0` | `0` | `1` | `0` | `0` | `0` | `1` | `0` | `0` | `0` | `0` | `1` | `0` |


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


