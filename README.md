# Lab9CA
Control Unit for a processor
module Control_unit (clk,rst,inst,stat_flag,ALU_OP,imm_sel,WB,ALUsrc,PCsrc,RW, MRW);

input clk,rst;
input [31:0] inst;
input [3:0] stat_flag;
output reg WB, ALUsrc, PCsrc,RW, MRW;
output reg [3:0] ALU_OP;
output reg [1:0] imm_sel;
	
always @(inst or rst)
	if(rst==0)
	case(inst[6:0])
	
		7'b0110011:begin 
		ALUsrc=1'b0;
		RW= 1'b1;
		MRW=1'b0;
		WB=1'b0;
		PCsrc=1'b0;
		imm_sel= 2'b00;
		ALU_OP={inst[30],inst[14:12]};
		end 
		
		7'b0010011:begin  
		ALUsrc=1'b1;
		RW= 1'b1;
		MRW=1'b0;
		WB=1'b1;
		PCsrc=1'b0;
		imm_sel= 2'b01;
		ALU_OP={inst[30],inst[14:12]};
		end 
		
		7'b0000011:begin 
		ALUsrc=1'b1;
		RW= 1'b1;
		MRW=1'b0;
		WB=1'b1;
		PCsrc=1'b0;
		imm_sel= 2'b01;
		ALU_OP=4'b0000;
		end 
		
		7'b0100011:begin 
		ALUsrc=1'b1;
		RW= 1'b1;
		MRW=1'b0;
		WB=1'b1;
		PCsrc=1'b0;
		imm_sel= 2'b10;
		ALU_OP=4'b0000;
		end 
		
		7'b1100011:begin 
		ALUsrc=1'b0;
		RW=1'b0;
		MRW=1'b1;
		WB=1'b1;
		imm_sel=2'b11;
		ALU_OP=4'b1000;
		
		case (inst[14:12])
		3'b000:begin
		PCsrc=stat_flag[0]?1'b1:1'b0;
		end
		3'b100:begin 
		PCsrc=stat_flag[1]? 1'b1:1'b0;
		end 
		
		endcase
		end
		endcase
		else
		
		case(inst[6:0])
		7'b0000000:begin
		ALUsrc=1'b0;
		RW= 1'b0;
		MRW=1'b0;
		WB=1'b0;
		PCsrc=1'b0;
		imm_sel= 2'b00;
		ALU_OP=4'b0000;
		end
		endcase 
	endmodule 
  
  
  Test bench\
  module Control_unit_tb ();
	reg clk,rst;
	reg [31:0] inst;
	reg [3:0] stat_flag;
	wire [3:0] ALU_OP;
	wire [1:0] imm_sel;
	wire WB,ALUsrc,PCsrc,RW,MRW;
	
	Control_unit test (.clk(clk),.rst(rst),.inst(inst),.stat_flag(stat_flag),.ALU_OP(ALU_OP),.imm_sel(imm_sel),.WB(WB),.ALUsrc(ALUsrc),.PCsrc(PCsrc),.RW(RW),.MRW(MRW));
	
	initial begin 
	clk=0;
	forever #5 clk= ~clk;
	
	end 
	
	initial begin 
	
	rst=1;
	#10 rst=0;
	stat_flag = 4'b0000;
	inst=32'b00000000011100110000001010110011;
	#10 inst=32'b0000000101000101000001110010011;
	#10 inst=32'b00000000011000111010000000100011;
	#10 inst=32'b00000000000000111010111000000011;
	#10 inst=32'b00000001110000111000111101100011;
	
	end 
endmodule 
