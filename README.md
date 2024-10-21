# PIPO

Design code:


module pipo (
    input [3:0] data_in,
    input clk,          
    input rst,         
    output reg [3:0] data_out
);

always @(posedge clk or posedge rst) begin
    if (rst) begin
        data_out <= 4'b0000;
    end else begin
        data_out <= data_in; 
    end
end
endmodule

Test bench:

module tb_pipo;
reg [3:0] data_in;
reg clk;
reg rst;
wire [3:0] data_out;
pipo uut (
    .data_in(data_in),
    .clk(clk),
    .rst(rst),
    .data_out(data_out)
);
always begin
    #5 clk = ~clk;
end
initial begin
    clk = 0;
    rst = 0;
    #10;
    rst = 1;
    #10;
    rst = 0;
    #10;
    data_in = 4'b1010;
    #10;
    data_in = 4'b1100;
    #10;
    rst = 1;
    #10;
    rst = 0;
    data_in = 4'b0111;
    #10
    #20;
    $stop;
end
initial begin
    $monitor("At time %0d: data_in = %b, rst = %b, data_out = %b", $time, data_in, rst, data_out);
end

endmodule

