```verilog
module b2g_converter #(parameter N =4) ( input wire [N-1:0]Binary,
                                        output wire [N-1:0]Gray);
  
  genvar i;
  generate 
    for(i=0;i<N-1;i=i+1)begin
      assign Gray[i] = Binary[i] ^ Binary[i+1];
    end
  endgenerate
  
  assign Gray[N-1] = Binary[N-1];
  
endmodule
