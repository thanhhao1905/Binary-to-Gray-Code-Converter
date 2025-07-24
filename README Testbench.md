```verilog
`timescale 1ps/1ps

module tbb2g_converter;
  parameter N=4;
  
  reg [N-1:0]Binary;
  wire [N-1:0]Gray;
  reg [N-1:0]exp_Gray;
  integer err =0;
  integer i;
  
  b2g_converter #(N) DUT(Binary,Gray);
  
  initial begin
    $monitor("time=%0t,Binary=%b,Gray=%b | exp_Gray=%b",$time,Binary,Gray,exp_Gray);
    
    for(i=0;i<2**N;i=i+1)begin
      Binary = i;
      exp_Gray = Binary ^ (Binary >> 1);
      #5;
      check(Gray,exp_Gray);
    end
    
    if(err==0) begin
      $display("-----------");
      $display("Test Pass");
      $display("-----------");
    end else begin
      $display("-----------");
      $display("Test False with %d error",err);
      $display("-----------");
    end
    
    
    $finish;
  end
  
  
  task check(input [N-1:0]Gray, input [N-1:0]exp_Gray);
    begin
      if(Gray!==exp_Gray)begin
        $display("[CHECK] ERROR");
        err=err+1;
      end else begin
        $display("[CHECK] MATCHING");
      end
    end
  endtask
  
  initial begin
    $dumpfile("dump.vcd");
    $dumpvars;
  end
endmodule
