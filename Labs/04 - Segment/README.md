## Preparation

| **Hex** | **Inputs** | **A** | **B** | **C** | **D** | **E** | **F** | **G** |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| 0 | 0000 | 0 | 0 | 0 | 0 | 0 | 0 | 1 |
| 1 | 0001 | 1 | 0 | 0 | 1 | 1 | 1 | 1 |
| 2 | 0010 | 0 | 0 | 1 | 0 | 0 | 1 | 0 |
| 3 | 0011 | 0 | 0 | 0 | 0 | 1 | 1 | 0 |
| 4 | 0100 | 1 | 0 | 0 | 1 | 1 | 0 | 0 |
| 5 | 0101 | 0 | 1 | 0 | 0 | 1 | 0 | 0 |
| 6 | 0110 | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
| 7 | 0111 | 0 | 0 | 0 | 1 | 1 | 1 | 1 |
| 8 | 1000 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 9 | 1001 | 0 | 0 | 0 | 0 | 0 | 1 | 0 |
| A | 1010 | 0 | 0 | 0 | 1 | 0 | 0 | 0 |
| b | 1011 | 1 | 1 | 0 | 0 | 0 | 0 | 0 |
| C | 1100 | 0 | 1 | 1 | 0 | 0 | 0 | 1 |
| d | 1101 | 1 | 0 | 0 | 0 | 0 | 1 | 0 |
| E | 1110 | 0 | 1 | 1 | 0 | 0 | 0 | 0 |
| F | 1111 | 0 | 1 | 1 | 1 | 0 | 0 | 0 |

## Part 2 - seg7



**hex_7seg.vhdl**

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity hex_7seg is
   Port ( 
        hex_i : in STD_LOGIC_VECTOR (4-1 downto 0);
        seg_o : out STD_LOGIC_VECTOR (7-1 downto 0)
         );
end hex_7seg;

architecture Behavioral of hex_7seg is

begin

    p_7seg_decoder : process(hex_i)
    begin
        case hex_i is
            when "0000" =>
                seg_o <= "0000001";     -- 0
            
            when "0001" =>
                seg_o <= "1001111";     -- 1
            
            when "0010" =>
                seg_o <= "0010010";     -- 2
            
            when "0011" =>
                seg_o <= "0000110";     -- 3
            
            when "0100" =>
                seg_o <= "1001100";     -- 4
            
            when "0101" =>
                seg_o <= "0100100";     -- 5
            
            when "0110" =>
                seg_o <= "0100000";     -- 6
            
            when "0111" =>
                seg_o <= "0001111";     -- 7
            
            when "1000" =>
                seg_o <= "0000000";     -- 8
            
            when "1001" =>
                seg_o <= "0000010";     -- 9
            
            when "1010" =>
                seg_o <= "0001000";     -- A
            
            when "1011" =>
                seg_o <= "1100000";     -- b
            
            when "1100" =>
                seg_o <= "0110001";     -- C
            
            when "1101" =>
                seg_o <= "1000010";     -- d    
            
            when "1110" =>
                seg_o <= "0110000";     -- E
            
            when others =>
                seg_o <= "0111000";     -- F
        end case;
    end process p_7seg_decoder;


end Behavioral;

```

**tb_hex_7seg**

```vhdl

library IEEE;
use IEEE.STD_LOGIC_1164.ALL;


entity tb_hex_7seg is
--  Port ( );
end tb_hex_7seg;

architecture Behavioral of tb_hex_7seg is

    signal s_hex : STD_LOGIC_VECTOR (4-1 downto 0);
    signal s_seg : STD_LOGIC_VECTOR (7-1 downto 0);


begin
    uut_hex_7seg  : entity work.hex_7seg
        port map (
        
        hex_i => s_hex,
        seg_o => s_seg
        );

    p_stimulus : process
    begin
        report "Stimulus process started" severity note;
        
        s_hex <= "0000"; wait for 100 ns; --0
        
        s_hex <= "0001"; wait for 100 ns; --1
        
        s_hex <= "0010"; wait for 100 ns; --2
        
        s_hex <= "0011"; wait for 100 ns; --3
        
        s_hex <= "0100"; wait for 100 ns; --4
        
        s_hex <= "0101"; wait for 100 ns; --5
        
        s_hex <= "0110"; wait for 100 ns; --6
        
        s_hex <= "0111"; wait for 100 ns; --7
        
        s_hex <= "1000"; wait for 100 ns; --8
        
        s_hex <= "1001"; wait for 100 ns; --9
        
        s_hex <= "1010"; wait for 100 ns; --A
        
        s_hex <= "1011"; wait for 100 ns; --b
        
        s_hex <= "1100"; wait for 100 ns; --C
        
        s_hex <= "1101"; wait for 100 ns; --d
        
        s_hex <= "1110"; wait for 100 ns; --E
        
        s_hex <= "1111"; wait for 100 ns; --F
        
           
        report "Stimulus process finished" severity note;
    end process p_stimulus;

end Behavioral;

```

![Simulace seg7](images/simulation seg7.PNG)

## Part 3 - top


**top.vhdl**

```vhdl

library IEEE;
use IEEE.STD_LOGIC_1164.ALL;


entity top is
    Port ( 
           SW : in STD_LOGIC_VECTOR (4-1 downto 0);     --input binary data
           CA : out std_logic;                          --Catody
           CB : out std_logic;
           CC : out std_logic;
           CD : out std_logic;
           CE : out std_logic;
           CF : out std_logic;
           CG : out std_logic;
           
           LED : out std_logic_vector(8-1 downto 0); -- LED indicator
           AN  : out std_logic_vector (8-1 downto 0) -- 
           );
end top;

architecture Behavioral of top is

begin

    hex2seg : entity work.hex_7seg
        port map(
            hex_i => SW,
            
            seg_o(6) => CA,
            seg_o(5) => CB,
            seg_o(4) => CC,
            seg_o(3) => CD,
            seg_o(2) => CE,
            seg_o(1) => CF,
            seg_o(0) => CG
        );

        AN <= b"1111_0111";
        
         -- Display input value
    LED(3 downto 0) <= SW;

    -- Turn LED(4) on if input value is equal to 0, ie "0000"
    -- WRITE YOUR CODE HERE
     LED(4)  <= '1' when (SW = "0000") else '0';
  
    -- Turn LED(5) on if input value is greater than "1001"
    -- WRITE YOUR CODE HERE
     LED(5)  <= '1' when (SW > "1001") else '0';
    
    -- Turn LED(6) on if input value is odd, ie 1, 3, 5, ...
    -- WRITE YOUR CODE HERE
    LED(6) <= '1' when (SW = "0001" or SW = "0011" or SW = "0101" or SW = "0111" or SW = "1001" or SW = "1011" or SW = "1101" or SW = "1111") else '0';
    
    -- Turn LED(7) on if input value is a power of two, ie 1, 2, 4, or 8
    -- WRITE YOUR CODE HERE
    LED(7)  <= '1' when (SW = "0001" or SW = "0010" or SW = "0100" or SW = "1000") else '0';

end Behavioral;

```

**tb_top.vhdl**

```vhdl

library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity tb_top is

end tb_top;

architecture Behavioral of tb_top is

    signal s_SW : STD_LOGIC_VECTOR (4 - 1 downto 0); -- Input binary data
    signal s_CA : STD_LOGIC; -- 	Cathod A
    signal s_CB : STD_LOGIC; -- 	Cathod B
    signal s_CC : STD_LOGIC; -- 	Cathod C
    signal s_CD : STD_LOGIC; -- 	Cathod D
    signal s_CE : STD_LOGIC; -- 	Cathod E
    signal s_CF : STD_LOGIC; -- 	Cathod F
    signal s_CG : STD_LOGIC; -- 	Cathod G
        
    signal s_LED : STD_LOGIC_VECTOR (8 - 1 downto 0); -- LED indicators
    signal s_AN  : STD_LOGIC_VECTOR (8 - 1 downto 0); -- Common anode signals to individual displays

begin

    uut_top : entity work.top
        port map(
            SW           => s_SW,
            CA           => s_CA,
            CB           => s_CB,
            CC           => s_CC,
            CD           => s_CD,
            CE           => s_CE,
            CF           => s_CF,
            CG           => s_CG,
            LED          => s_LED,
            AN           => s_AN
        );

p_stimulus : process
    begin

        report "Stimulus process started" severity note;

        s_SW <= "0000"; wait for 100 ns;
        
        s_SW <= "0001"; wait for 100 ns;
        
        s_SW <= "0010"; wait for 100 ns;
        
        s_SW <= "0011"; wait for 100 ns;
        
        s_SW <= "0100"; wait for 100 ns;
       
        s_SW <= "0101"; wait for 100 ns;
        
        s_SW <= "0110"; wait for 100 ns;
        
        s_SW <= "0111"; wait for 100 ns;
        
        s_SW <= "1000"; wait for 100 ns;
        
        s_SW <= "1001"; wait for 100 ns;
        
        s_SW <= "1010"; wait for 100 ns;
        
        s_SW <= "1011"; wait for 100 ns;
        
        s_SW <= "1100"; wait for 100 ns;
        
        s_SW <= "1101"; wait for 100 ns;
        
        s_SW <= "1110"; wait for 100 ns;
        
        s_SW <= "1111"; wait for 100 ns;

        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;

end Behavioral;

```
