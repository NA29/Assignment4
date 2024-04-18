library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity Booth_Mult is
    Port (
        In_1, In_2 : in std_logic_vector(7 downto 0);
        clk        : in std_logic;
        ready      : in std_logic;
        done       : out std_logic;
        S          : out std_logic_vector(15 downto 0)
    );
end Booth_Mult;

architecture Behavioral of Booth_Mult is
    signal A       : signed(7 downto 0);
    signal Q       : std_logic_vector(8 downto 0);
    signal M       : std_logic_vector(7 downto 0);
    signal done2   : std_logic; --since we can't use done directly for internal logic
    signal counter : integer range 0 to 8 := 0; -- ensure initialization and proper range

begin
    process(clk)
        variable AmM : signed(7 downto 0); --A minus M, signed for arithmetic operations
        variable ApM : signed(7 downto 0); --A plus M, signed for arithmetic operations

    begin
        if rising_edge(clk) then
            if ready = '1' then
                --initialisation
                A <= (others => '0');
                Q <= In_2 & '0';  -- Concatenation to initialize with one extra bit
                M <= std_logic_vector(signed(In_1)); -- cast to signed if operations are signed
                counter <= 0;
                done <= '0';
                done2 <= '0';

            elsif ready = '0' and done2 = '0' then
                -- Process the bits based on Booth's algorithm
                case Q(1 downto 0) is
                    when "00" | "11" =>
                        -- Right shift A and Q
                        A <= '0' & A(7 downto 1);
                        Q <= A(0) & Q(8 downto 1);
                    when "10" =>
                        -- Subtract M from A, then right shift
                        AmM := signed(A) - signed(M);
                        A <= '0' & AmM(7 downto 1);
                        Q <= AmM(0) & Q(8 downto 1);
                    when "01" =>
                        -- Add M to A, then right shift
                        ApM := signed(A) + signed(M);
                        A <= '0' & ApM(7 downto 1);
                        Q <= ApM(0) & Q(8 downto 1);
                    when others =>
                        -- Handle unexpected cases, usually none
                end case;

                -- Increment counter to track progress
                counter <= counter + 1;
                if counter >= 8 then
                    done <= '1';
                    done2 <= '1';
                end if;

            end if;
        end if;
        -- Concatenate A and Q to form the final product output, S
        S <= std_logic_vector(A) & Q(8 downto 1);
    end process;
end architecture;
