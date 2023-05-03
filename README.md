Download Link: https://assignmentchef.com/product/solved-cs161l-lab-3-the-datapath-control-and-alu-control-units
<br>
For this lab you will be building the control units for a MIPS processor. See the next figure.




There are two control units. The​<em> main control unit</em>​ manages the datapath. It receives an opcode input from the currently executing instructions and based on this opcode it configures the datapath accordingly. A truth table for the unit functionality (shown below) can be found in the slides of CS161. The table (read vertically) shows the output for ​R-format, lw, sw ​and beq​ instructions, additionally, you will need to implement the immediate type (​addi,subi​) instructions. To do this, you will trace through the datapath (shown above) to determine which control lines will need to be set.







<table width="624">

 <tbody>

  <tr>

   <td width="92"><strong>Control </strong></td>

   <td width="110"><strong>Signal name </strong></td>

   <td width="115"><strong>R-format </strong></td>

   <td width="77"><strong>lw </strong></td>

   <td width="76"><strong>sw </strong></td>

   <td width="80"><strong>beq </strong></td>

   <td width="74"><strong>imm </strong></td>

  </tr>

  <tr>

   <td rowspan="6" width="92"><strong> </strong><strong> </strong><strong> </strong><strong> </strong><strong>Inputs </strong></td>

   <td width="110">Op5</td>

   <td width="115">0</td>

   <td width="77">1</td>

   <td width="76">1</td>

   <td width="80">0</td>

   <td width="74">0</td>

  </tr>

  <tr>

   <td width="110">Op4</td>

   <td width="115">0</td>

   <td width="77">0</td>

   <td width="76">0</td>

   <td width="80">0</td>

   <td width="74">0</td>

  </tr>

  <tr>

   <td width="110">Op3</td>

   <td width="115">0</td>

   <td width="77">0</td>

   <td width="76">1</td>

   <td width="80">0</td>

   <td width="74">1</td>

  </tr>

  <tr>

   <td width="110">Op2</td>

   <td width="115">0</td>

   <td width="77">0</td>

   <td width="76">0</td>

   <td width="80">1</td>

   <td width="74">0</td>

  </tr>

  <tr>

   <td width="110">Op1</td>

   <td width="115">0</td>

   <td width="77">1</td>

   <td width="76">1</td>

   <td width="80">0</td>

   <td width="74">0</td>

  </tr>

  <tr>

   <td width="110">Op0</td>

   <td width="115">0</td>

   <td width="77">1</td>

   <td width="76">1</td>

   <td width="80">0</td>

   <td width="74">0</td>

  </tr>

  <tr>

   <td rowspan="9" width="92"><strong> </strong><strong> </strong><strong> </strong><strong> </strong><strong> </strong><strong> </strong><strong> </strong><strong>Outputs </strong></td>

   <td width="110">RegDst</td>

   <td width="115">1</td>

   <td width="77">0</td>

   <td width="76">X</td>

   <td width="80">X</td>

   <td width="74"> </td>

  </tr>

  <tr>

   <td width="110">ALUSrc</td>

   <td width="115">0</td>

   <td width="77">1</td>

   <td width="76">1</td>

   <td width="80">0</td>

   <td width="74"> </td>

  </tr>

  <tr>

   <td width="110">MemtoReg</td>

   <td width="115">0</td>

   <td width="77">1</td>

   <td width="76">X</td>

   <td width="80">X</td>

   <td width="74"> </td>

  </tr>

  <tr>

   <td width="110">RegWrite</td>

   <td width="115">1</td>

   <td width="77">1</td>

   <td width="76">0</td>

   <td width="80">0</td>

   <td width="74"> </td>

  </tr>

  <tr>

   <td width="110">MemRead</td>

   <td width="115">0</td>

   <td width="77">1</td>

   <td width="76">0</td>

   <td width="80">0</td>

   <td width="74"> </td>

  </tr>

  <tr>

   <td width="110">MemWrite</td>

   <td width="115">0</td>

   <td width="77">0</td>

   <td width="76">1</td>

   <td width="80">0</td>

   <td width="74"> </td>

  </tr>

  <tr>

   <td width="110">Branch</td>

   <td width="115">0</td>

   <td width="77">0</td>

   <td width="76">0</td>

   <td width="80">1</td>

   <td width="74"> </td>

  </tr>

  <tr>

   <td width="110">ALUOp1</td>

   <td width="115">1</td>

   <td width="77">0</td>

   <td width="76">0</td>

   <td width="80">0</td>

   <td width="74"> </td>

  </tr>

  <tr>

   <td width="110">ALUOp0</td>

   <td width="115">0</td>

   <td width="77">0</td>

   <td width="76">0</td>

   <td width="80">1</td>

   <td width="74"> </td>

  </tr>

 </tbody>

</table>

<strong>Table 1. </strong>​The control function for the simple one-clock implementation. Read each column vertically. I.e. if the ​Op[5:0]​ is ​000000​, the ​RegDst​ would be ‘​1​’, ​ALUSrc​ would be ‘0​ ​’, etc. You will need to fill in the ​imm​ column. Based on Figure D.2.4 from the book.

The second control unit manages the ​<em>ALU</em>​. It receives an ALU opcode from the datapath controller and the ‘​Funct Field​’ from the current instruction. With these, the ALU controller decides what operation the ALU is to perform. The following figures from the CS161 slides give an idea of the inputs and outputs of the ALU controller.

<table width="624">

 <tbody>

  <tr>

   <td width="104"> </td>

   <td colspan="3" width="288"><strong>Input </strong></td>

   <td colspan="2" width="232"><strong>Output </strong></td>

  </tr>

  <tr>

   <td width="104"><strong>Instruction Opcode </strong></td>

   <td width="69"><strong>ALUOp </strong></td>

   <td width="120"><strong>Instruction operation </strong></td>

   <td width="99"><strong>Funct field </strong></td>

   <td width="128"><strong>Desired ALU action </strong></td>

   <td width="105"><strong>ALU select input </strong></td>

  </tr>

  <tr>

   <td width="104">lw</td>

   <td width="69">00</td>

   <td width="120">Load word</td>

   <td width="99">XXXXXX</td>

   <td width="128">add</td>

   <td width="105">0010</td>

  </tr>

  <tr>

   <td width="104">sw</td>

   <td width="69">00</td>

   <td width="120">Store word</td>

   <td width="99">XXXXXX</td>

   <td width="128">add</td>

   <td width="105">0010</td>

  </tr>

  <tr>

   <td width="104">beq</td>

   <td width="69">01</td>

   <td width="120">Branch equal</td>

   <td width="99">XXXXXX</td>

   <td width="128">subtract</td>

   <td width="105">0110</td>

  </tr>

  <tr>

   <td width="104">R-type</td>

   <td width="69">10</td>

   <td width="120">add</td>

   <td width="99">100000</td>

   <td width="128">add</td>

   <td width="105">0010</td>

  </tr>

  <tr>

   <td width="104">R-type</td>

   <td width="69">10</td>

   <td width="120">subtract</td>

   <td width="99">100010</td>

   <td width="128">subtract</td>

   <td width="105">0110</td>

  </tr>

  <tr>

   <td width="104">R-type</td>

   <td width="69">10</td>

   <td width="120">AND</td>

   <td width="99">100100</td>

   <td width="128">and</td>

   <td width="105">0000</td>

  </tr>

  <tr>

   <td width="104">R-type</td>

   <td width="69">10</td>

   <td width="120">OR</td>

   <td width="99">100101</td>

   <td width="128">or</td>

   <td width="105">0001</td>

  </tr>

  <tr>

   <td width="104">R-type</td>

   <td width="69">10</td>

   <td width="120">NOR</td>

   <td width="99">100111</td>

   <td width="128">nor</td>

   <td width="105">1100</td>

  </tr>

  <tr>

   <td width="104">R-type</td>

   <td width="69">10</td>

   <td width="120">Set on less than</td>

   <td width="99">101010</td>

   <td width="128">Set on less than</td>

   <td width="105">0111</td>

  </tr>

 </tbody>

</table>

<strong>ALU select bits based on ALUop, and Funct field</strong>

<table width="623">

 <tbody>

  <tr>

   <td colspan="2" width="170"><strong>ALUOp </strong></td>

   <td colspan="6" width="347"><strong>Funct field </strong></td>

   <td rowspan="2" width="106"><strong> </strong><strong>Operation </strong></td>

  </tr>

  <tr>

   <td width="86"> </td>

   <td width="84"> </td>

   <td width="47"> </td>

   <td width="56"> </td>

   <td width="63"> </td>

   <td width="66"> </td>

   <td width="60"> </td>

   <td width="55"> </td>

  </tr>

  <tr>

   <td width="86"><strong>ALUOp1 </strong></td>

   <td width="84"><strong>ALUOp0 </strong></td>

   <td width="47"><strong>F5 </strong></td>

   <td width="56"><strong>F4 </strong></td>

   <td width="63"><strong>F3 </strong></td>

   <td width="66"><strong>F2 </strong></td>

   <td width="60"><strong>F1 </strong></td>

   <td width="55"><strong>F0 </strong></td>

   <td width="106"> </td>

  </tr>

  <tr>

   <td width="86">0</td>

   <td width="84">0</td>

   <td width="47">X</td>

   <td width="56">X</td>

   <td width="63">X</td>

   <td width="66">X</td>

   <td width="60">X</td>

   <td width="55">X</td>

   <td width="106">0010</td>

  </tr>

  <tr>

   <td width="86">X</td>

   <td width="84">1</td>

   <td width="47">X</td>

   <td width="56">X</td>

   <td width="63">X</td>

   <td width="66">X</td>

   <td width="60">X</td>

   <td width="55">X</td>

   <td width="106">0110</td>

  </tr>

  <tr>

   <td width="86">1</td>

   <td width="84">X</td>

   <td width="47">X</td>

   <td width="56">X</td>

   <td width="63">0</td>

   <td width="66">0</td>

   <td width="60">0</td>

   <td width="55">0</td>

   <td width="106">0010</td>

  </tr>

  <tr>

   <td width="86">1</td>

   <td width="84">X</td>

   <td width="47">X</td>

   <td width="56">X</td>

   <td width="63">0</td>

   <td width="66">0</td>

   <td width="60">1</td>

   <td width="55">0</td>

   <td width="106">0110</td>

  </tr>

  <tr>

   <td width="86">1</td>

   <td width="84">X</td>

   <td width="47">X</td>

   <td width="56">X</td>

   <td width="63">0</td>

   <td width="66">1</td>

   <td width="60">0</td>

   <td width="55">0</td>

   <td width="106">0000</td>

  </tr>

  <tr>

   <td width="86">1</td>

   <td width="84">X</td>

   <td width="47">X</td>

   <td width="56">X</td>

   <td width="63">0</td>

   <td width="66">1</td>

   <td width="60">0</td>

   <td width="55">1</td>

   <td width="106">0001</td>

  </tr>

  <tr>

   <td width="86">1</td>

   <td width="84">X</td>

   <td width="47">X</td>

   <td width="56">X</td>

   <td width="63">1</td>

   <td width="66">0</td>

   <td width="60">1</td>

   <td width="55">0</td>

   <td width="106">0111</td>

  </tr>

  <tr>

   <td width="86">1</td>

   <td width="84">X</td>

   <td width="47">X</td>

   <td width="56">X</td>

   <td width="63">0</td>

   <td width="66">1</td>

   <td width="60">1</td>

   <td width="55">1</td>

   <td width="106">1100</td>

  </tr>

 </tbody>

</table>

<strong>Truth Table for ALU Control </strong>

<h1>Deliverables</h1>

For this lab, you are expected to build and test both the datapath using the template provided (​controlUnit.v​) and ALU control (​aluControlUnit.v​) units. The target processor architecture will only support a subset of the MIPS instructions, listed below. You only have to offer control for these instructions. Signal values can be found within your textbook (and in the images above).




<ul>

 <li>add, addu, addi</li>

 <li>sub, subu slt</li>

 <li>not*, nor</li>

 <li>or</li>

 <li>and</li>

 <li>lw, sw</li>

 <li>beq</li>

</ul>




Notice that for the ​addu​ it is sufficient to generate the same control signals as the ​add operation.

* ​not​ is ​<u>not</u>​ an instruction, it is a ​<em>pseudo-op</em>​, which means it can be implemented using other operations. Think about how you would implement it using the other operations.

<h1>Architecture Case Study</h1>

For the lab this week you are also expected to perform a simple case study. It is meant to show how important understanding a computer’s architecture is, and the compiler is when developing efficient code. For this study, you are to compare and analyze the execution time of the two programs given <u>​</u><a href="https://elearn.ucr.edu/files/326474/download?download_frd=1">here</a>​. You should run a number of experiments varying the input size from 100 to 30,000.  Based on the results you are to write a report of your findings. The report should contain a graph of your data and a useful analysis of it. You should draw conclusions based on your findings. Reports that simply restate what is in the graph will not get credit.  To make it clear, make sure you used the concepts you have learned so far in 161 and 161L when explaining the differences in performance.  If a confusing or fuzzy explanation is given you will get low or no marks.  The report should be in PDF format.