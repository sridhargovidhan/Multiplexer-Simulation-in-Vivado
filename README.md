# SIMULATION AND IMPLEMENTATION OF MULTIPLEXER
## Name: Sridhar G
## 212223060271

## AIM
To design and simulate a 4:1 Multiplexer (MUX) using Verilog HDL in four different modeling styles—Gate-Level, Data Flow, Behavioral, and Structural—and to verify its functionality through a testbench using the Vivado 2023.1 simulation environment. The experiment aims to understand how different abstraction levels in Verilog can be used to describe the same digital logic circuit and analyze their performance.

## APPARATUS REQUIRED
- **Vivado 2023.1**

## Procedure
### Launch Vivado
- Open Vivado 2023.1 from the Start menu or desktop icon.

### Create a New Project
1. Click **"Create Project"**.
2. Enter the project name (e.g., `Mux4_to_1`) and choose a location. Click **Next**.
3. Select **RTL Project** and click **Next**.
4. Add Verilog files (e.g., `mux4_to_1_gate.v`, `mux4_to_1_dataflow.v`) and enable **"Copy sources into project"**. Click **Next**.
5. Skip the Constraints step and proceed.
6. Select a default FPGA part (e.g., `xc7a35ticsg324-1L`). Click **Next**, then **Finish**.

### Add Source Files (if needed)
- In the Sources window, right-click on **"Design Sources"** → **Add Sources**.
- Add Verilog files and the testbench (`mux4_to_1_tb.v`).

### Check Syntax
- In the Flow Navigator, under **Synthesis**, click **"Run Synthesis"**.
- Correct any errors or warnings, then re-run the synthesis.

### Simulate the Design
- In the Flow Navigator, under **Simulation**, click **"Run Behavioral Simulation"**.

### Analyze Simulation Results
- View waveform signals (e.g., `S1`, `S0`, `A`, `B`, `C`, `D`, etc.).
- Use the waveform controls to zoom, scroll, and analyze results.

### Adjust Simulation Time
- Click **"Simulation"** → **"Simulation Settings"** to modify the run time (e.g., 1000ns).

### Generate Report
- Right-click the simulation window → **"Export Simulation Results"**.

### Save and Document
- Click **File** → **Save Project**.
- Capture screenshots of the waveform window for your lab report.

### Close Simulation
- Go to **"Simulation"** → **"Close Simulation"** when finished.

---

## Logic Diagram
![image](https://github.com/user-attachments/assets/d4ab4bc3-12b0-44dc-8edb-9d586d8ba856)

---

## Truth Table
![image](https://github.com/user-attachments/assets/c850506c-3f6e-4d6b-8574-939a914b2a5f)

---

## Verilog Code

### 4:1 MUX Gate-Level Implementation
```verilog
module mux41(s,a,b,c,d,y);
input [0:1]s;
input a,b,c,d;
output y;
wire [0:5]w;
not g1(w[0],s[0]);
not g2(w[1],s[1]);
and g3(w[2],w[0],w[1],a);
and g4(w[3],w[0],s[1],b);
and g5(w[4],s[0],w[1],c);
and g6(w[5],s[0],s[1],d);
or o1(y,w[2],w[3],w[4],w[5]);
endmodule
![WhatsApp Image 2025-03-15 at 17 25 02_6a8ab26e](https://github.com/user-attachments/assets/e81824be-b428-45c3-8918-ea5a31891070)

```
## Simulated Output Gate Level Modelling

![WhatsApp Image 2025-03-15 at 17 25 02_d9bed6be](https://github.com/user-attachments/assets/ebcd17c1-1363-41da-8d16-46bae3b7b9d2)


### 4:1 MUX Data Flow Implementation
```verilog
module dataflow_mux(s,a,b,c,d,y);
input [0:1]s;
input a,b,c,d;
output y;
wire [0:3]w;
assign w[0]=~s[0]&~s[1]&a;
assign w[1]=~s[0]&s[1]&b;
assign w[2]=~s[1]&s[0]&c;
assign w[3]=s[0]&s[1]&d;
assign y=w[1]|w[2]|w[3]|w[0];
endmodule

```
## Simulated Output Data Flow Modelling

![WhatsApp Image 2025-03-15 at 17 15 48_11b3bd7f](https://github.com/user-attachments/assets/6309f06d-b3a5-4db8-94d5-a72b7439b03e)


### 4:1 MUX Behavioral Implementation
```verilog
module behi_mux(
    input wire [1:0] sel,   
    input wire [3:0] d,     
    output reg y           
);

always @(*) begin
    if (sel == 2'b00)
        y = d[0];
    else if (sel == 2'b01)
        y = d[1];
    else if (sel == 2'b10)
        y = d[2];
    else if (sel == 2'b11)
        y = d[3];
    else
        y = 1'b0; 
end

endmodule

```
## Simulated Output Behavioral Modelling

![WhatsApp Image 2025-03-15 at 17 09 03_df5afcba](https://github.com/user-attachments/assets/41f6e556-abee-4e62-a048-5ebbbb6ca51b)



### 4:1 MUX Structural Implementation

![image](https://github.com/user-attachments/assets/eea81c2c-7dfa-43aa-9cea-1ab4ed54db6c)


```verilog
module mux2_to1 (
    input a,b,
    input s,
    output y
);
    assign y = s?b:a;
endmodule

module mux_struct(a,b,c,d,s,y);
input a,b,c,d;
input [1:0]s;
wire [1:0]w;
output y;
mux2_to1 mux0(a,b,s[0],w[0]);
mux2_to1 mux1(c,d,s[0],w[1]);
mux2_to1 mux2(w[0],w[1],s[1],y);

endmodule

```
## Simulated Output Structural Modelling
![WhatsApp Image 2025-03-16 at 14 36 03_8c1bf5cc](https://github.com/user-attachments/assets/e7385df9-002c-4644-a593-ec089c8e0e00)



### Testbench Implementation
```verilog
`timescale 1ns / 1ps

module mux4_to_1_tb;
    reg A, B, C, D, S0, S1;
    wire Y_gate, Y_dataflow, Y_behavioral, Y_structural;

    mux4_to_1_gate uut_gate (.A(A), .B(B), .C(C), .D(D), .S0(S0), .S1(S1), .Y(Y_gate));
    mux4_to_1_dataflow uut_dataflow (.A(A), .B(B), .C(C), .D(D), .S0(S0), .S1(S1), .Y(Y_dataflow));
    mux4_to_1_behavioral uut_behavioral (.A(A), .B(B), .C(C), .D(D), .S0(S0), .S1(S1), .Y(Y_behavioral));
    mux4_to_1_structural uut_structural (.A(A), .B(B), .C(C), .D(D), .S0(S0), .S1(S1), .Y(Y_structural));

    initial begin
        A = 0; B = 0; C = 0; D = 0; S0 = 0; S1 = 0;

        #10 {S1, S0, A, B, C, D} = 6'b00_0001;
        #10 {S1, S0, A, B, C, D} = 6'b01_0010;
        #10 {S1, S0, A, B, C, D} = 6'b10_0100;
        #10 {S1, S0, A, B, C, D} = 6'b11_1000;
        #10 $stop;
    end

    initial begin
        $monitor("Time=%0t | S1=%b S0=%b | Y_gate=%b | Y_dataflow=%b | Y_behavioral=%b | Y_structural=%b",
                 $time, S1, S0, Y_gate, Y_dataflow, Y_behavioral, Y_structural);
    end
endmodule
```
### SAMPLE OUTPUT
```verilog
Time=0 | S1=0 S0=0 | Y_gate=0 | Y_dataflow=0 | Y_behavioral=0 | Y_structural=0
Time=10 | S1=0 S0=1 | Y_gate=0 | Y_dataflow=0 | Y_behavioral=0 | Y_structural=0
Time=20 | S1=1 S0=0 | Y_gate=0 | Y_dataflow=0 | Y_behavioral=0 | Y_structural=0
```
**CONCLUSION**

In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural.The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions.

