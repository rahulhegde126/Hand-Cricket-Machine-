module hand_cricket_game_tb;

    // Testbench signals
    logic clk;
    logic reset;
    logic [2:0] player_run;
    logic btn_play;
    logic [7:0] leds;
    logic out_flag;

    // Instantiate the hand_cricket_game module
    hand_cricket_game dut (
        .clk(clk),
        .reset(reset),
        .player_run(player_run),
        .btn_play(btn_play),
        .leds(leds),
        .out_flag(out_flag)
    );

    // Clock generation: 10ns period
    always #5 clk = ~clk;

    // Task to simulate a single button press and score a run
    task play_run(input logic [2:0] run);
        begin
            player_run = run;
            btn_play = 1;
            #10;  // Wait for one clock cycle with button pressed
            btn_play = 0;
            #10;  // Wait for one clock cycle with button released
        end
    endtask

    initial begin
        // Initialize signals
        clk = 0;
        reset = 0;
        player_run = 0;
        btn_play = 0;

        // Simulation begins
        $display("Starting hand cricket game testbench...");

        // Reset the game
        reset = 1;
        #10;
        reset = 0;

        // Test: Play a few runs without hitting the "out" condition
        play_run(3);   // Score 3 runs
        $display("Score: %0d, Out Flag: %b", leds, out_flag);

        play_run(5);   // Score 5 runs
        $display("Score: %0d, Out Flag: %b", leds, out_flag);

        play_run(2);   // Score 2 runs
        $display("Score: %0d, Out Flag: %b", leds, out_flag);

        // Test: Trigger "out" condition by matching random_run with player_run
        // Since random_run is pseudo-random, you may need to test multiple times or set it manually if possible.
        // For testing, assume random_run is known to be 3 for simplicity in this scenario.
        play_run(3);   // Assuming this matches random_run, it should trigger "out"
        $display("Score: %0d, Out Flag: %b (should be 1 if out)", leds, out_flag);

        // Reset the game to start over after "out"
        reset = 1;
        #10;
        reset = 0;

        play_run(4);   // Score 4 runs after reset
        $display("Score: %0d, Out Flag: %b", leds, out_flag);

        $display("End of testbench.");
        $finish;
    end
