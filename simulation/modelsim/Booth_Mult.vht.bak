library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity Booth_Mult is
end entity;



architecture sim of Booth_Mult is

	component Booth_Mult is
		Port(In_1	: in std_logic_vector(7 downto 0);
			  In_2	: in std_logic_vector(7 downto 0);
			  clk		: in std_logic;
			  ready	: in std_logic;
			  done	: out std_logic;
			  S		: out std_logic_vector(15 downto 0) );
	end component;


	constant clkFrequency  : integer := 100e6; --100 MHz
	constant clkPeriod		: time	 := 100 ms / clkFrequency;
	
	signal In_1_tb, In_2_tb		: std_logic_vector(7 downto 0);
	signal clk_tb					: std_logic := '1';
	signal ready_tb, done_tb	: std_logic;
	signal S_tb						: std_logic_vector(15 downto 0);
	
	
begin
	

	DUT : Booth_Mult
		port map(In_1 => In_1_tb,
					In_2 => In_2_tb,
					clk => clk_tb,
					ready => ready_tb,
					S => S_tb );
					
		
	-- generating clock
	clk_tb <= not clk_tb after clkPeriod/2;
	
	process
	begin
		--test 1
		In_1_tb <= "00000001";
		In_2_tb <= "00000010";
		ready_tb <= '1';
		wait for clkPeriod;
		ready_tb <= '0';
			
		wait until done_tb = '1';
		

		wait;
				
	end process;
	
end architecture;