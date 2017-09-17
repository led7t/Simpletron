# Simpletron
//Laura E Dean
//Spring 2018
//Version 3.0


//Simpletron is a simple machine that takes user input to complete an operation.
//The Simpletron runs programs written in the only language it directly understands: Simpletron Machine Language, or SML for short.
//All the information in the Simpletron is handled in terms of words. A word is a signed four-digit decimal number.
//Simpletron is equipped with a 100-word memory, and these words are referenced by their location numbers.
//The first two digits of each SML instruction are the operation code specifying the operation to be performed.
//The last two digits of an SML instruction are the operand—the address of the memory location containing the word to which the operation applies.



//Updates from version 1 to version 2:
//Ability to read in a program from a text file rather than input it via the program
//Memory dump after the completion of a program


//Upates from version 2 to version 3:
//New operations REMAINDER, EXPONENTIATION, and NEWLINE added.





using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Simpletron
{
    class Program
    {

        public static void Main(string[] args)
        {

            //memeory array of 100 elements
            int[] memory = new int[100];

            //operation codes
            const int READ = 10;          //read a word from the keyboard into a specific location in memory
            const int WRITE = 11;         //write a word from a specific location in memory to the screen
            const int LOAD = 20;          //load a word from a specific location in memory into the accumultor
            const int STORE = 21;         //store a word from the accumulattor into a specific location in memory
            const int ADD = 30;           //add a word from a specific location in memory to the word in the accumulator
            const int SUBTRACT = 31;      //subtract a word from a specific location in memory from the word in the accumulator
            const int DIVIDE = 32;        //divide a word from a specific location in memory into the word in the accumulator
            const int MULTIPLY = 33;      //multiply a word from a specific location in memory by the word in the accumulator
            const int BRANCH = 40;        //branch to a specific location in memory
            const int BRANCHNEG = 41;     //branch to a specific location in memory if the accumulator is negative
            const int BRANCHZERO = 42;    //branch to a specific location in memory if the accumulator is zero
            const int HALT = 43;          //halt-the program has completed its task
            const int REMAINDER = 50;     //allows the simulator to perform remainder calculations
            const int EXPONENTIATION = 51;//allows the simulator to perform exponentiation calculations
            const int NEWLINE = 60;       //allows output of a newline



            //various variables
            bool array;                  //temp variable the user input(string) gets parsed into
            int count = 0;               //used as a counter
            int operand = 0;             //indicates the memory location on which the current operation operates
            int operationCode = 0;       //indicates the operation currently being performed
            int instructionCount = 0;    //keeps track of the location in memory that contains the instruction being performed
            int instructionRegister = 0; //contans the next instruction to be performed from the memory
            int accumulator = 0;         //special register into which information is put before the Simpletron uses it in calculations/ect
            string input = "";           //string containing user file

            Console.WriteLine("*** Welcome to Simpletron! ***");
            Console.WriteLine("*** Your program file is loading ***" + "\n");



            //take in user values via .txt file
            System.IO.StreamReader file = new System.IO.StreamReader("C:\\Users\\Laptop\\Desktop\\test.txt");
            while ((input = file.ReadLine()) != null)
            {
                array = int.TryParse(input, out memory[count]);
                count++;
            }


            //lets the user know their input was successfully loaded and the program is going to begin execution
            Console.WriteLine("*** Program loading completed ***");
            Console.WriteLine("*** Program execution begins ***" + "\n");


            while (instructionCount < count)
            {
                //fetches the contents of the i location from memory
                instructionRegister = memory[instructionCount];
                //extraction of the operation code from the instruction register
                operationCode = instructionRegister / 100;
                //extraction of the operand from the instruction register
                operand = instructionRegister % 100;

                //using the current operation code and determining what operation to use
                switch (operationCode)
                {

                    //read a word from the keyboard into a specific location in memory
                    case READ:
                        {
                            Console.WriteLine("Enter an integer: ");
                            string temp = Console.ReadLine();
                            int.TryParse(temp, out memory[operand]);
                            instructionCount++;
                            break;
                        }

                    //write a word from a specific location in memory to the screen
                    case WRITE:
                        {
                            Console.WriteLine("Result: ");
                            Console.WriteLine(memory[operand] + "\n");
                            instructionCount++;
                            break;
                        }

                    //load a word from a specific location in memory into the accumultor
                    case LOAD:
                        {
                            accumulator = memory[operand];
                            instructionCount++;
                            break;
                        }

                    //store a word from the accumulattor into a specific location in memory
                    case STORE:
                        {
                            memory[operand] = accumulator;
                            instructionCount++;
                            break;
                        }

                    //add a word from a specific location in memory to the word in the accumulator
                    case ADD:
                        {
                            accumulator += memory[operand];
                            instructionCount++;
                            break;
                        }

                    //subtract a word from a specific location in memory from the word in the accumulator
                    case SUBTRACT:
                        {
                            accumulator -= memory[operand];
                            instructionCount++;
                            break;
                        }

                    //divide a word from a specific location in memory into the word in the accumulator
                    case DIVIDE:
                        {
                            if (memory[operand] == 0)
                            {

                                Console.WriteLine("ERROR: Can not divide by zero!" + "\n");
                                Environment.Exit(0);
                                break;
                            }

                            else
                                accumulator /= memory[operand];
                            instructionCount++;
                            break;
                        }

                    //multiply a word from a specific location in memory by the word in the accumulator
                    case MULTIPLY:
                        {
                            accumulator *= memory[operand];
                            instructionCount++;
                            break;
                        }

                    //branch to a specific location in memory
                    case BRANCH:
                        {
                            instructionCount = operand;
                            break;
                        }

                    //branch to a specific location in memory if the accumulator is negative
                    case BRANCHNEG:
                        {
                            if (accumulator < 0)
                            {
                                instructionCount = operand;
                            }
                            break;
                        }

                    //branch to a specific location in memory if the accumulator is zero
                    case BRANCHZERO:
                        {
                            if (accumulator == 0)
                            {
                                instructionCount = operand;
                            }

                            break;
                        }

                    //Halt-the program has completed its task
                    case HALT:
                        {
                            Console.WriteLine("HALT" + "\n");
                            accumulator = 0;
                            operationCode = 0;
                            operand = 0;
                            instructionCount = count;
                            Console.WriteLine("*** Simpletron execution terminated ***" + "\n");
                            break;
                        }

                    //finds the remainder 
                    case REMAINDER:
                        {
                            if (memory[operand] == 0)
                            {

                                Console.WriteLine("ERROR: Can not divide by zero!");
                                Environment.Exit(0);
                                break;
                            }

                            else
                                accumulator %= memory[operand];
                            instructionCount++;
                            break;
                        }

                    //exponentition operation
                    case EXPONENTIATION:
                        {
                            int result = 1;
                            while (memory[operand] != 0)
                            {
                                result *= accumulator;
                                --memory[operand];
                            }
                            accumulator = result;
                            instructionCount++;
                            break;
                        }

                    //operation that gives a blank line
                    case NEWLINE:
                        {
                            Console.WriteLine();
                            instructionCount++;
                            break;
                        }
                    //if the user enters unvalid input print out an error message 
                    default:
                        {
                            Console.WriteLine("ERROR: not valid input");
                            Environment.Exit(0);
                            break;
                        }

                }


            }
            //Print to screen the values of instructions and data values at the moment execution terminated
            Console.WriteLine("REGISTERS:");
            Console.WriteLine("accumulator: +" + accumulator);
            Console.WriteLine("instructionCount: " + instructionCount);
            Console.WriteLine("instructionRegister: +" + instructionRegister);
            Console.WriteLine("operationCode: " + operationCode);
            Console.WriteLine("operand: " + operand);
            Console.WriteLine();
            Console.WriteLine();

            Console.WriteLine("  MEMORY:");

            //memory dump
            int counter = 0;

            //top row 0-9
            for (int i = 0; i < 10; i++)
            {
                Console.Write("{0}", "   " + i + "   ");
            }

            Console.WriteLine();

            for (int x = 0; x < 100; x += 10)
            {
                //first column 0-90
                if (counter % 10 <= 0)
                {
                    Console.Write("{0}", counter.ToString("00"));

                    //memory
                    for (int y = x; y < x + 10; y++)
                    {
                        if (memory[y] < 0)
                        {
                            Console.Write("{0}", (memory[y] * (-1)).ToString(" -0000") + " ");
                        }
                        else
                        {
                            Console.Write("{0}", memory[y].ToString(" +0000") + " ");
                        }
                        counter++;

                    }
                    Console.WriteLine();
                }

            }
            Console.WriteLine();
        }
    }
}
