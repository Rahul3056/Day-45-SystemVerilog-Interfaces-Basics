### **Day 45: SystemVerilog Interfaces – Basics**

### **1. What is an Interface in SystemVerilog?**

An **interface** in SystemVerilog is a special construct that **bundles together related signals** and optionally, associated **modports**, tasks, or functions. It helps reduce wiring complexity and enhances **modularity** and **reusability** in both design and testbench environments.

---

### **2. Why Use Interfaces?**

Without interfaces, connecting many signals between modules (e.g., DUT and testbench) results in long and error-prone port lists.

Using an interface:

* Combines signals into one named object.
* Promotes clean and consistent connectivity.
* Supports encapsulation and abstraction.

---

### **3. Interface Syntax**

```systemverilog
interface <interface_name> ();
    // Declare interface signals here
endinterface
```

You can declare wires, logic, parameters, clocks, resets, tasks, functions, and even modports inside an interface.

---

### **4. Example: Basic Interface for a Bus**

```systemverilog
interface bus_if;
    logic clk;
    logic rst_n;
    logic [7:0] addr;
    logic [7:0] data;
    logic read;
    logic write;
endinterface
```

---

### **5. Using an Interface in a Module**

#### **Module using the interface (e.g., DUT):**

```systemverilog
module dut(bus_if bus);  // Connecting interface

    always_ff @(posedge bus.clk or negedge bus.rst_n) begin
        if (!bus.rst_n)
            bus.data <= 8'd0;
        else if (bus.write)
            bus.data <= bus.addr + 8'd1;
    end

endmodule
```

---

### **6. Using the Interface in a Testbench**

```systemverilog
module tb;

    // Instantiate the interface
    bus_if bus();

    // Instantiate DUT and connect interface
    dut d1(.bus(bus));

    // Clock generation
    initial begin
        bus.clk = 0;
        forever #5 bus.clk = ~bus.clk;
    end

    // Stimulus
    initial begin
        bus.rst_n = 0; #10;
        bus.rst_n = 1;

        bus.write = 1;
        bus.addr = 8'h12; #10;

        bus.write = 0; #10;

        $finish;
    end

endmodule
```

---

### **7. Modports (Preview)**

Modports define **roles** (read/write permissions) for different modules using the same interface. We will cover **modports** in a future topic.

---

### **8. Summary**

| Concept     | Explanation                            |
| ----------- | -------------------------------------- |
| Interface   | Group of related signals               |
| Reusability | Interfaces promote modular design      |
| Connection  | Passed as a single port to modules     |
| Abstraction | Can include tasks, functions, modports |
