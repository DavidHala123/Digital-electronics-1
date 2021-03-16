## Preparation


   | **Time interval** | **Number of clk periods** | **Number of clk periods in hex** | **Number of clk periods in binary** |
   | :-: | :-: | :-: | :-: |
   | 2&nbsp;ms | 200 000 | `x"3_0d40"` | `b"0011_0000_1101_0100_0000"` |
   | 4&nbsp;ms | 400 000 | `x"6_1A80"` | `b"0110_0001_1010_1000_0000"` |
   | 10&nbsp;ms | 1 000 000 | `x"F_4240"` | `b"1111_0100_0010_0100_0000"` |
   | 250&nbsp;ms | 25 000 000 | `x"17D_7840"` | `b"0001_0111_1101_0111_1000_0100_0000"` |
   | 500&nbsp;ms | 50 000 000 | `x"2FA_F080"` | `b"0010_1111_1010_1111_0000_1000_0000"` |
   | 1&nbsp;sec | 100 000 000 | `x"5F5_E100"` | `b"0101_1111_0101_1110_0001_0000_0000"` |

## Part 2 - Bidirectional counter



**p_cnt_up_down**

```vhdl
    p_cnt_up_down : process(clk)
    begin
        if rising_edge(clk) then
        
            if (reset = '1') then               -- Synchronous reset
                s_cnt_local <= (others => '0'); -- Clear all bits

            elsif (en_i = '1') then       -- Test if counter is enabled
                -- TEST COUNTER DIRECTION HERE
                if (cnt_up_i = '1') then
                    s_cnt_local <= s_cnt_local + 1;
                else
                    s_cnt_local <= s_cnt_local - 1;
                end if;
            end if;
        end if;
    end process p_cnt_up_down;

```

**tb_cmt_up_down**

```vhdl

library ieee;
use ieee.std_logic_1164.all;

entity tb_cnt_up_down is
    -- Entity of testbench is always empty
end entity tb_cnt_up_down; 

architecture testbench of tb_cnt_up_down is

    -- Number of bits for testbench counter
    constant c_CNT_WIDTH         : natural := 4;
    constant c_CLK_100MHZ_PERIOD : time    := 10 ns;

    --Local signals
    signal s_clk_100MHz : std_logic;
    signal s_reset      : std_logic;
    signal s_en         : std_logic;
    signal s_cnt_up     : std_logic;
    signal s_cnt        : std_logic_vector(c_CNT_WIDTH - 1 downto 0);

begin
    -- Connecting testbench signals with cnt_up_down entity
    -- (Unit Under Test)
    uut_cnt : entity work.cnt_up_down
        generic map(
            g_CNT_WIDTH  => c_CNT_WIDTH
        )
        port map(
            clk      => s_clk_100MHz,
            reset    => s_reset,
            en_i     => s_en,
            cnt_up_i => s_cnt_up,
            cnt_o    => s_cnt
        );

    p_clk_gen : process
    begin
        while now < 750 ns loop         -- 75 periods of 100MHz clock
            s_clk_100MHz <= '0';
            wait for c_CLK_100MHZ_PERIOD / 2;
            s_clk_100MHz <= '1';
            wait for c_CLK_100MHZ_PERIOD / 2;
        end loop;
        wait;
    end process p_clk_gen;

    p_reset_gen : process
    begin
        s_reset <= '0';
        wait for 30 ns;
        
        -- Reset activated
        s_reset <= '1';
        wait for 60 ns;
       
        s_reset <= '0';
        wait;
    end process p_reset_gen;

    p_stimulus : process
    begin
        report "Stimulus process started" severity note;

        -- Enable counting
        s_en     <= '1';
        
        -- Change counter direction
        s_cnt_up <= '1';
        wait for 420 ns;
        s_cnt_up <= '0';
        wait for 210 ns;

        -- Disable counting
        s_en     <= '0';

        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;

end architecture testbench;

```

**Analysis**
![Analysis]("images/05_analysis.PNG ")
![Analysis]("images/05_analysis_blizko.PNG")

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
