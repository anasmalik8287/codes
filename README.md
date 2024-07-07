# code of vending machine
module vending_machine(
    output reg nw_pa,
    input [1:0] coin,
    input clk,
    input rst
);

    reg [1:0] state, next_state;

    // State Definitions
    localparam [1:0] S0 = 2'b00;
    localparam [1:0] S5 = 2'b01;
    localparam [1:0] S10 = 2'b10;
    localparam [1:0] S15 = 2'b11;

    // State Register
    always @(posedge clk or posedge rst) begin
        if (rst)
            state <= S0;
        else
            state <= next_state;
    end

    // Next State Logic
    always @(*) begin
        case (state)
            S0: begin
                if (coin == 2'b00)
                    next_state = S0;
                else if (coin == 2'b01)
                    next_state = S5;
                else if (coin == 2'b10)
                    next_state = S10;
                else
                    next_state = S0;
            end
            S5: begin
                if (coin == 2'b00)
                    next_state = S5;
                else if (coin == 2'b01)
                    next_state = S10;
                else if (coin == 2'b10)
                    next_state = S15;
                else
                    next_state = S5;
            end
            S10: begin
                if (coin == 2'b00)
                    next_state = S10;
                else if (coin == 2'b01)
                    next_state = S15;
                else if (coin == 2'b10)
                    next_state = S15;
                else
                    next_state = S10;
            end
            S15: begin
                next_state = S0;
            end
            default: begin
                next_state = S0;
            end
        endcase
    end

    // Output Logic
    always @(*) begin
        case (state)
            S0, S5, S10: nw_pa = 1'b0;
            S15: nw_pa = 1'b1;
            default: nw_pa = 1'b0;
        endcase
    end

endmodule
