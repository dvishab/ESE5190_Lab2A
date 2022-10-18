# ESE5190_Lab2A

## **3.2**:

1.Why is bit-banging impractical on your laptop, despite it having a much faster processor than the RP2040?

Bit-banging is hard and even though the processor is much faster than RP2040, the processor isn’t really designed for that. For "bit-banging" one needs to sit the processor in a carefully timed loop, often painstakingly written in assembly, trying to make sure the GPIO reading and writing happens on the exact cycle required and this requires really hard work if indeed possible at all. ). Also, your processor is now busy doing the "bit-banging", and cannot be used for other tasks. If the processor is interrupted even for a few microseconds to attend to one of the hard peripherals it is also responsible for, this can be fatal to the timing of any bit-banged protocol. And due to this, it is not advisable to use bit-banging.

2.What are some cases where directly using the GPIO might be a better choice than using the PIO hardware?

For running general purpose softwares, GPIO is used as PIO is not capable of running general purpose softwares.

3.How do you get data into a PIO state machine?

From the transmit FIFO buffer, we take one data item and place it in the output shift register (OSR). This data moves one word at a time (32 bits) from the FIFO to the OSR. The OSR is capable of shifting its data out, one or more bits at a time to further destinations. 

4.How do you get data out of a PIO state machine?

The state machine needs to be told which GPIO or GPIOs to set as output. There are four different pin groups which are used by different instructions in different situations. The GPIO also needs to be told that PIO is in control of it (GPIO function select). PIO can drive the ‘output enable’ line up and down programmatically using certain instructions.

5.How do you program a PIO state machine

The RP2040 has two PIO blocks, each of them with four state machines. Each PIO block has a 32-slot instruction memory which is visible to the four state machines in the block. Our program needs to be loaded into this instruction memory before any of our state machines can run the program. Once the program is loaded, we find a free state machine and tell it to run our program. We can order multiple state machines to run the same program, or tell each state machine to run a different program. The state machine stalls if we don’t provide any data in the TX FIFO, for which it is waiting. We need to write a function to push the data into the TX FIFO. In this way, we can successfully program a PIO state machine.



6.In the example, which low-level C SDK function is directly responsible for telling the PIO to set the LED to a new color? How is this function accessed from the main “application” code?

In the example, the pio_sm_put_blocking() function is responsible at the lowest level for telling the PIO to set the LED to a new color. It takes its input from the pixel_rgb. It falls within the put_pixel() function. The put_pixel() function takes its arguments from another function urgb_u32(), which defines the RGB values of the colors to be displayed on the LED.
This function is being accessed using the puts() function. We pass pattern_table[] as an argument for this function. The pattern table lets us access various patterns in which we want our LEDs to blink. In each of these pattern functions, the put_pixel function is being calles. This leads to the color changing action which is required for our program to function.

7.What role does the pioasm “assembler” play in the example, and how does this interact with CMake?

pioasm tool is used to convert the assembler code to binary. We use the CMake function to invoke the pioasm and add the generated header to the include path of our target. The actual function being used it pico_generate_pio_header(TARGETPIO_FILE).


## **3.3**
