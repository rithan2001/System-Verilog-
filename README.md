# System-Verilog-
Udemy

##  Using initial block to generate stimuli 
```ruby
`timescale 1ns / 1ps

module tb();
 
/////global signal clk , rst
  
  reg clk;
  reg rst;
  
  reg [3:0] temp;
  
  //1. Initialized Global Variable
  initial begin
    clk = 1'b0;
    rst = 1'b0;
  end
  ////2. Random signal for data/ control 
  
  initial begin
    rst = 1'b1;
    #30;
    rst = 1'b0;
  end
  
  
  initial begin
  temp = 4'b0100;
  #10;
  temp = 4'b1100;
  #10; 
  temp = 4'b0011;
  #10;  
  end
  
  
  ///////3. System Task at the start of simulation
  initial begin
    $dumpfile("dump.vcd");
    $dumpvars;
  end
  /////4. Analyzing Values of variable on COnsole
  initial begin
    $monitor("Temp : %0d at time : %0t", temp, $time);
  end
  
  ///5. Stop simulation by forcefully calling $finish
  initial begin
    #200;
    $finish();
  end
  
endmodule
```

## always block in test bench 

```ruby
timescale 1ns / 1ps
 
module tb();
 
  
  reg clk; //initial value = X
  
  reg clk50;
  reg clk25 = 0;  ///initialize variable
  
 
  initial begin
    clk = 1'b0;
    rst = 1'b0;
    clk50 = 0;
    
  end
 
  
  always #5 clk = ~clk; //100 MHz
  
  always #10 clk50 = ~clk50;  ///50 MHz
  
  always #20 clk25 = ~clk25;  ///25 MHz
  
  
  
  
 
  initial begin
    $dumpfile("dump.vcd");
    $dumpvars;
  end
 
 
  initial begin
    #200;
    $finish();
  end
  
endmodule
```
