## 1.Preparation


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


## 2.D latch

**p_d_latch.vhdl**

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity d_latch is
  Port ( 
         en     : in std_logic;
         arst   : in std_logic;
         d      : in std_logic;
         q      : out std_logic;
         q_bar  : out std_logic
  
  );
end d_latch;

architecture Behavioral of d_latch is

begin

    p_d_latch : process (d, arst, en)
    begin
        if (arst = '1') then
            q <= '0';
            q_bar <= '1';
       
        elsif (en = '1') then
            q <= d;
            q_bar <= not d;   
        
        end if; 
          
    end process p_d_latch;  
     
end Behavioral;


```

**tb_d_latch.vhdl**

```vhdl

library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity tb_d_latch is
 -- Port (  );
end tb_d_latch;

architecture Behavioral of tb_d_latch is
        signal s_en     :  std_logic;
        signal s_arst   :  std_logic;
        signal s_d      :  std_logic;
        signal s_q      :  std_logic;
        signal s_q_bar  :  std_logic;

begin
uut_d_latch: entity work.d_latch
    port map(
        en    => s_en,   
        arst  => s_arst, 
        d     => s_d,    
        q     => s_q,    
        q_bar => s_q_bar
);
p_reset_gen : process
    begin
        s_arst <= '0';
        wait for 12 ns;
        
        -- Reset activated
        s_arst <= '1';
        wait for 80 ns;
       
        
        s_arst <= '0';
        wait for 150 ns;
        
        -- Reset activated
        s_arst <= '1';
        wait for 60 ns;
       
        s_arst <= '0';
        wait;
    end process p_reset_gen;
    
    p_stimulus : process
    begin
        report "Stimulus process started" severity note;
            s_d <= '0';
            s_en <= '0';
            
            
            assert (s_q = '0')
            report "nesouhlasí" severity error;
            
            
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            
            assert (s_q = '0' and s_q_bar = '1')
            report "nesouhlasí" severity error;
            
            
            s_en <= '1';

            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            
            s_en <= '0';
            
            
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';    
            
            s_en <= '1';
            
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0'; 
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0'; 
    
    report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;

end Behavioral;
```

**Simulation tb_d_latch**
![Sim(tb_d_latch)](Images/Sim(tb_d_latch).PNG)

## 3.Flip Flops

**p_d_ff_arts**

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;

-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;

entity d_ff_arst is
  Port ( 
         clk     : in std_logic;
         arst   : in std_logic;
         d      : in std_logic;
         q      : out std_logic;
         q_bar  : out std_logic
         );
end d_ff_arst;

architecture Behavioral of d_ff_arst is

begin
     p_d_latch : process (arst, clk)
    begin
        if (arst = '1') then
            q <= '0';
            q_bar <= '1';
       
        elsif rising_edge(clk) then
            q <= d;
            q_bar <= not d;   
        
        end if; 
          
    end process p_d_latch;  

end Behavioral;
```

**p_d_ff_rts**

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;

-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;

entity d_ff_arst is
  Port ( 
         clk     : in std_logic;
         arst   : in std_logic;
         d      : in std_logic;
         q      : out std_logic;
         q_bar  : out std_logic
         );
end d_ff_arst;

architecture Behavioral of d_ff_arst is

begin
     p_d_latch : process (arst, clk)
    begin
        if (arst = '1') then
            q <= '0';
            q_bar <= '1';
       
        elsif rising_edge(clk) then
            q <= d;
            q_bar <= not d;   
        
        end if; 
          
    end process p_d_latch;  

end Behavioral;
```

**p_jk_ff_rst**

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;

-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;

entity jk_ff_rst is
  Port ( 
         clk    : in std_logic;
         rst    : in std_logic;
         j      : in std_logic;
         k      : in std_logic;
         q      : out std_logic;
         q_bar  : out std_logic
  );
end jk_ff_rst;

architecture Behavioral of jk_ff_rst is

    signal s_q : std_logic;

begin
 p_d_latch : process (clk)
    begin
        if rising_edge(clk) then
           
            if (rst = '1')then
                s_q <= '0';
            
            else
                
                if (j = '0' and k = '0')then
                    s_q <= s_q;
               
                elsif (j = '0' and k = '1')then  
                    s_q <= '0';
              
                elsif (j = '1' and k = '0')then 
                    s_q <= '1';
              
                elsif (j = '1' and k = '1')then  
                    s_q <= not s_q;                 
                
                end if;
             
            end if;
             
        end if; 
          
    end process p_d_latch; 

    q <= s_q;
    q_bar <= not s_q;

end Behavioral;
```

**p_t_ff_rst**

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;

-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;

entity t_ff_rst is
  Port ( 
         clk     : in std_logic;
         rst   : in std_logic;
         t      : in std_logic;
         q      : out std_logic;
         q_bar  : out std_logic
  );
end t_ff_rst;

architecture Behavioral of t_ff_rst is

    signal s_q : std_logic;

begin
 p_t_ff_rst : process (clk)
    begin
        if rising_edge(clk) then
           
            if (rst = '1')then
                s_q <= '0';
            
            else
                
                if (t = '0')then
                    s_q <= s_q;
               
                elsif (t = '1')then  
                    s_q <= not s_q;                 
                
                end if;
             
            end if;
             
        end if; 
          
    end process p_t_ff_rst; 

    q <= s_q;
    q_bar <= not s_q;


end Behavioral;
```
**Simulation tb_d_ff_arst**
![Sim(tb_d_ff_ars)](Images/Sim(tb_d_ff_arst).PNG)
**Simulation tb_ik_ff_rst**
![Sim(tb_ik_ff_rst)](Images/Sim(tb_ik_ff_rst).PNG)
**Simulation tb_jk_ff_rst**
![Sim(tb_jk_ff_rst)](Images/Sim(tb_jk_ff_rst).PNG)
**Simulation tb_t_ff_rst**
![Sim(tb_t_ff_rst)](Images/Sim(tb_t_ff_rst).PNG)


**clk_gen**
```vhdl
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
        s_arst <= '0';
        wait for 12 ns;
        
        -- Reset activated
        s_arst <= '1';
        wait for 48 ns;
       
        
        s_arst <= '0';
        wait for 38 ns;
        
        -- Reset activated
        s_arst <= '1';
        wait for 60 ns;
       
        s_arst <= '0';
        wait;
    end process p_reset_gen;
 p_stimulus : process
    begin
        report "Stimulus process started" severity note;
            s_d <= '0';
            
            
            
            wait for 14ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            
            wait for 6ns;
            s_d <= '0';
            
            assert (s_q = '0' and s_q_bar = '1')
            report "nesouhlasí" severity error;
            
            
            wait for 4ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0'; 
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0'; 
            wait for 14ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0'; 
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0';
            wait for 10ns;
            s_d <= '1';
            wait for 10ns;
            s_d <= '0'; 
    
    report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;
end Behavioral;
```

