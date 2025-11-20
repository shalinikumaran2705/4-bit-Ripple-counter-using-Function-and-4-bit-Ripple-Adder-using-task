# 4-bit-Ripple-counter-using-Function-and-4-bit-Ripple-Adder-using-task

# Aim
To design and simulate a 4-bit-Ripple-counter-using-Function-and-4-bit-Ripple-Adder-using-task using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 environment. 

# Apparatus Required
Vivado 2023.1
# Procedure
1. Launch Vivado 2023.1
Open Vivado and create a new project.
2. Design the Verilog Code
Write the Verilog code for the seven-segment display, defining the logic that maps a 4-bit binary input to the corresponding segments (a to g) of the display.
3. Create the Testbench
Write a testbench to simulate the seven-segment display behavior. The testbench should apply various 4-bit input values and monitor the corresponding output.
4. Create the Verilog Files
Create both the design module and the testbench in the Vivado project.
5. Run Simulation
Run the behavioral simulation to verify the output. 
6. Observe the Waveforms
Analyze the output waveforms in the simulation window, and verify that the correct segments light up for each digit.
7. Save and Document Results
Capture screenshots of the waveform and save the simulation logs. These will be included in the lab report.

# Verilog Code
# 4 bit Ripple Adder using Task
```
`timescale 1ns/1ps 
module rca4(a,b,cin,sum,cout);
input [3:0] a,b;
input cin;
output reg [3:0] sum;
output reg cout;
reg [4:0] temp;
task ripple_add;
input [3:0] x,y;
input c_in;
output [3:0] s;
output c_out;
reg [4:0] t;
begin
t = x + y + c_in; 
s = t[3:0];
c_out = t[4];
end
endtask
always @(*) begin
ripple_add(a,b,cin,sum,cout);
end
endmodule
```


# Test Bench
```
module tb_rca4;
reg [3:0] a,b;
reg cin;
wire [3:0] sum;
wire cout;
rca4 uut(a,b,cin,sum,cout);
initial begin
$monitor("T=%0t | a=%b | b=%b | cin=%b | sum=%b | cout=%b",$time,a,b,cin,sum,cout);
a=4'b1010; b=4'b0101; cin=0;
#10;
a=4'b1111; b=4'b0001; cin=1;
#10;
a=4'b1001; b=4'b0110; cin=0;
#10;
a=4'b1110; b=4'b1111; cin=1;
#10;
end
```

# Output Waveform

<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/31e23200-9740-4de5-b97e-41109f1974b0" />


# 4 bit Ripple counter using Function
```
// 4-bit Ripple Counter using Function
module RippleCarryCounter_4bit (
    input clk,
    input reset,
    output [3:0] q
);
    reg [3:0] count;
    assign q = count;

    function [1:0] full_adder_func;
        input a_bit, b_bit, carry_in;
        begin
            full_adder_func[0] = a_bit ^ b_bit ^ carry_in;                   
            full_adder_func[1] = (a_bit & b_bit) | (carry_in & (a_bit ^ b_bit)); 
        end
    endfunction

    reg c1, c2, c3, c4; // Add one more carry variable

    always @(posedge clk or posedge reset) begin
        if (reset)
            count <= 4'b0000;
        else begin
            // Ripple increment by 1
            {c1, count[0]} = full_adder_func(count[0], 1'b1, 1'b0);
            {c2, count[1]} = full_adder_func(count[1], 1'b0, c1);
            {c3, count[2]} = full_adder_func(count[2], 1'b0, c2);
            {c4, count[3]} = full_adder_func(count[3], 1'b0, c3); 
        end
    end
endmodule
```

# Test Bench
```
module RippleCarryCounter_4bit_tb;
    reg clk, reset;
    wire [3:0] q;

    RippleCarryCounter_4bit uut (
        .clk(clk),
        .reset(reset),
        .q(q)
    );

    initial begin
        clk = 0;
        forever #5 clk = ~clk;
    end

    initial begin
        $monitor("Time=%0t | Reset=%b | Count=%b", $time, reset, q);
        reset = 1;
        #10 reset = 0;  
        #200 reset = 1;
        #10 reset = 0;
        #100 $stop;
    end
endmodule
```


# Output Waveform 

<img width="1464" height="921" alt="image" src="https://github.com/user-attachments/assets/1758b57f-e4d4-43fb-91dc-77c1bef47403" />



# Conclusion
In this experiment, a 4-bit-Ripple-counter-using-Function-and-4-bit-Ripple-Adder-using-task was successfully designed and simulated using Verilog HDL.
