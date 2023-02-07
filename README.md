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
## Timescale = time unit / time precision 
```ruby 
`timescale 1ns / 1ps   //10^3 -> 3
 
module tb();
 
  
 
  
  reg clk16 = 0;
  reg clk8 = 0;  ///initialize variable
  
 
   always #31.25 clk16 = ~clk16;
   always #62.5 clk8 = ~clk8;
  
 
 
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

## multiple clock with tasks
```ruby 
`timescale 1ns / 1ps
 
module tb();
 
  
  reg clk = 0; 
  reg clk50 = 0;
  
  always #5 clk = ~clk; //100 MHz
    
  /*
  real phase = 10;
  real ton = 5;
  real toff = 5;
  */
/*  
task clkgen(input real phase, input real ton, input real toff);  
  #phase;
  while(1) begin
  clk50 = 1;
  #ton;
  clk50 = 0;
  #toff;
  end
endtask
*/
 
 
task calc (input real freq_hz, input real duty_cycle, input real phase, output real pout, output real ton, output real toff);
pout = phase;
ton = (1.0 / freq_hz) * duty_cycle * 1000_000_000;
toff =  (1000_000_000 / freq_hz) - ton;
endtask
 
 
task clkgen(input real phase, input real ton, input real toff);  
  @(posedge clk);
  #phase;
  while(1) begin
  clk50 = 1;
  #ton;
  clk50 = 0;
  #toff;
  end
endtask
 
real phase;
real ton;
real toff;
 
 
initial begin
calc(100_000_000, 0.1, 2, phase, ton, toff);
clkgen(phase, ton, toff);
end
 
 
 
  initial begin
    #200;
    $finish();
  end
  
endmodule

```
