module hand_cricket_game (
    input  logic        clk,          
    input  logic        reset,       
    input  logic [2:0]  player1_run,  
    input  logic [2:0]  player2_run, 
    input  logic        input_gate,  
    input  logic        btn_play,     
    output logic [7:0]  leds,        
    output logic        out_flag    
);

    // Internal registers
    logic [2:0] p1_reg, p2_reg; // Latched runs
    logic [7:0] total_score;
    logic       btn_play_prev, gate_prev;

    // Edge detectors
    always_ff @(posedge clk or posedge reset) begin
        if (reset) begin
            btn_play_prev <= 0;
            gate_prev     <= 0;
        end else begin
            btn_play_prev <= btn_play;
            gate_prev     <= input_gate;
        end
    end

    wire gate_pulse = input_gate & ~gate_prev;  // Rising edge of gate
    wire play_pulse = btn_play   & ~btn_play_prev; // Rising edge of play button

    // Capture player inputs when gate is enabled
    always_ff @(posedge clk or posedge reset) begin
        if (reset) begin
            p1_reg <= 3'b000;
            p2_reg <= 3'b000;
        end
        else if (gate_pulse) begin
            p1_reg <= player1_run;
            p2_reg <= player2_run;
        end
    end

    // OUT detection and score update
    always_ff @(posedge clk or posedge reset) begin
        if (reset) begin
            total_score <= 8'b0;
            out_flag    <= 1'b0;
        end
        else if (play_pulse) begin
            if (p1_reg == p2_reg) begin
                out_flag    <= 1'b1;   // OUT condition
                total_score <= 8'b0;   // Reset score
            end
            else begin
                out_flag    <= 1'b0;
                total_score <= total_score + p1_reg;
            end
        end
    end

    assign leds = total_score;

endmodule

