using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Diagnostics;
using System.Threading;

namespace HydraConsole
{
    internal class Program
    {
        // ============================================================
        //  3D ASCII DESIGN HELPERS
        // ============================================================
        const int W_BOX = 52;

        static void W(string s, ConsoleColor c)
        {
            Console.ForegroundColor = c;
            Console.Write(s);
            Console.ResetColor();
        }

        // 3D logo with gradient + floor shadow
        static void Logo()
        {
            string[] logo =
            {
                @" _   _ __   __ ____   ____      _    ",
                @"| | | |\ \ / /|  _ \ |  _ \    / \   ",
                @"| |_| | \ V / | | | || |_) |  / _ \  ",
                @"|  _  |  | |  | |_| ||  _ <  / ___ \ ",
                @"|_| |_|  |_|  |____/ |_| \_\/_/   \_\"
            };
            ConsoleColor[] grad =
            {
                ConsoleColor.White, ConsoleColor.Cyan, ConsoleColor.Cyan,
                ConsoleColor.DarkCyan, ConsoleColor.DarkCyan
            };

            for (int i = 0; i < logo.Length; i++)
            {
                W("       " + logo[i], grad[i]);
                W("\n", ConsoleColor.Gray);
            }
            W("        " + new string('#', 37) + "\n", ConsoleColor.DarkGray);
            W("         " + new string('.', 35) + "\n", ConsoleColor.DarkGray);
        }

        // 3D box header with right + bottom shadow
        static void Header(string title, ConsoleColor c)
        {
            int w = W_BOX;
            int pad = (w - 2 - title.Length) / 2;
            string mid = new string(' ', pad) + title + new string(' ', w - 2 - pad - title.Length);
            string bar = new string('=', w - 2);

            W(" ." + bar + ".\n", c);
            W(" |" + mid + "|", c);
            W("#\n", ConsoleColor.DarkGray);
            W(" '" + bar + "'", c);
            W("#\n", ConsoleColor.DarkGray);
            W("  " + new string('#', w) + "\n", ConsoleColor.DarkGray);
        }

        // 3D button for menu items
        static void Button(string text, ConsoleColor c)
        {
            int w = 36;
            string bar = new string('-', w - 2);
            string mid = " " + text + new string(' ', Math.Max(0, w - 3 - text.Length));

            W("      +" + bar + "+\n", c);
            W("      |" + mid + "|", c);
            W("#\n", ConsoleColor.DarkGray);
            W("      +" + bar + "+", c);
            W("#\n", ConsoleColor.DarkGray);
        }

        static void Prompt()
        {
            W("\n  >> ", ConsoleColor.DarkGray);
            Console.Write("Enter your choice: ");
        }

        static void Main(string[] args)
        {
            Console.Title = "HYDRA CONSOLE";

            bool loginSuccess = LoginPage();
            if (loginSuccess == false)
            {
                return;
            }

            LoadingAnimation();
            WelcomePage();
            MainMenu();
            ExitProgram();


        }
        // WELCOMEPAGE
        static void WelcomePage()
        {
            Console.Clear();

            Console.WriteLine();
            Logo();
            Header("HYDRA CONSOLE", ConsoleColor.Green);

            Console.WriteLine("\n  Welcome to HYDRA Console!");
            Console.ReadLine();

            Console.WriteLine("  Press any key to continue...");
            Console.ReadKey();
        }

        //LOGINPAGE
        static bool LoginPage()
        {
            int attempts = 0;
            int maxAttempts = 3;

            while (attempts < maxAttempts)
            {
                Console.Clear();

                Console.WriteLine();
                Logo();
                Header("HYDRA LOGIN", ConsoleColor.Yellow);

                W("\n  [", ConsoleColor.DarkGray);
                W("USER", ConsoleColor.Cyan);
                W("] ", ConsoleColor.DarkGray);
                Console.Write("Enter Username: ");
                string username = Console.ReadLine();

                W("  [", ConsoleColor.DarkGray);
                W("PASS", ConsoleColor.Cyan);
                W("] ", ConsoleColor.DarkGray);
                Console.Write("Enter Password: ");
                string password = Console.ReadLine();

                if (username == "user" && password == "pass")
                {
                    W("\n  >> LOGIN SUCCESSFUL!\n", ConsoleColor.Green);
                    Console.ReadKey();

                    return true;
                }
                else
                {
                    attempts++;

                    W("\n  !! INVALID USERNAME OR PASSWORD!\n", ConsoleColor.Red);
                    Console.WriteLine("  Attempts remaining: " + (maxAttempts - attempts));

                    Console.ReadKey();
                }
            }

            W("\n  !! MAXIMUM ATTEMPTS REACHED!\n", ConsoleColor.Red);
            W("  !! ACCESS DENIED.\n", ConsoleColor.Red);

            Console.ReadKey();

            return false;
        }
        //LOADING ANIM
        static void LoadingAnimation()
        {
            Console.Clear();

            Console.WriteLine();
            Logo();
            Header("HYDRA CONSOLE", ConsoleColor.Green);

            W("\n  AUTHENTICATION SUCCESSFUL!\n", ConsoleColor.Green);
            Console.WriteLine("  Initializing HYDRA SYSTEM...\n");

            Console.Write("  Loading: ");
            W("[", ConsoleColor.DarkGray);

            for (int i = 0; i <= 20; i++)
            {
                W("#", i < 7 ? ConsoleColor.DarkCyan : (i < 14 ? ConsoleColor.Cyan : ConsoleColor.Green));
                System.Threading.Thread.Sleep(100);
            }

            W("]", ConsoleColor.DarkGray);

            W("\n\n  >> SYSTEM READY!\n", ConsoleColor.Green);

            Console.ReadKey();
        }
        // MAIN MENU
        static void MainMenu()
        {
            while (true)
            {
                Console.Clear();

                Console.WriteLine();
                Logo();
                Header("CONSOLE SYSTEM  //  MAIN MENU", ConsoleColor.Green);

                Console.WriteLine();
                Button("[1]  BASIC", ConsoleColor.Cyan);
                Button("[2]  INTERMEDIATE", ConsoleColor.Yellow);
                Button("[3]  ENTERTAINMENT", ConsoleColor.Magenta);
                Button("[0]  EXIT", ConsoleColor.Red);

                Prompt();
                string choice = Console.ReadLine();

                if (choice == "1")
                {
                    BasicMenu();
                }
                else if (choice == "2")
                {
                    IntermediateMenu();
                }
                else if (choice == "3")
                {
                    EntertainmentMenu();
                }
                else if (choice == "0")
                {
                    ExitProgram();


                    return;
                }
                else
                {
                    W("\n  !! Invalid selection!\n", ConsoleColor.Red);
                    Console.ReadKey();
                }
            }
        }
        // BASIC CATEGORY MENU
        static void BasicMenu()
        {
            while (true)

            {
                Console.Clear();

                Console.WriteLine();
                Header("BASIC CATEGORY", ConsoleColor.Green);

                Console.WriteLine();
                Button("[1]  BILL CALCULATOR", ConsoleColor.Cyan);
                Button("[2]  BASIC CALCULATOR", ConsoleColor.Cyan);
                Button("[0]  BACK TO MAIN MENU", ConsoleColor.Red);

                Prompt();
                string choice = Console.ReadLine();

                if (choice == "1")
                {
                    ElectricityBillCalculator();
                }
                else if (choice == "2")
                {
                    BasicCalculator();
                }
                else if (choice == "0")
                {
                    return;
                }
                else
                {
                    W("\n  !! Invalid selection!\n", ConsoleColor.Red);
                    Console.ReadKey();
                }

            }
        }
        // ELECTRICITY BILL CALCULATOR
        static void ElectricityBillCalculator()
        {
            while (true)
            {
                Console.Clear();

                Console.WriteLine();
                Header("ELECTRICITY BILL CALCULATOR", ConsoleColor.Yellow);

                Console.Write("\n  Enter Customer Name: ");
                string name = Console.ReadLine();

                Console.Write("  Enter Electricity Consumption (kWh): ");
                double kwh = Convert.ToDouble(Console.ReadLine());

                double rate = 12.00;
                double bill = kwh * rate;

                W("\n  .=========== BILL SUMMARY ===========.\n", ConsoleColor.Cyan);
                Console.WriteLine("   Customer Name: " + name);
                Console.WriteLine("   Consumption: " + kwh + " kWh");
                Console.WriteLine("   Rate per kWh: PHP " + rate);
                W("   Total Bill: PHP " + bill + "\n", ConsoleColor.Green);

                if (bill >= 2000)
                {
                    W("   Reminder: High electricity bill!\n", ConsoleColor.Red);
                }
                else
                {
                    W("   Reminder: Thank you for using our service!\n", ConsoleColor.Green);
                }

                W("  '====================================='", ConsoleColor.Cyan);
                W("#\n", ConsoleColor.DarkGray);
                W("   " + new string('#', 37) + "\n", ConsoleColor.DarkGray);

                Console.Write("\n  Calculate another bill? (Y/N): ");
                string again = Console.ReadLine().ToUpper();

                if (again == "N")
                {
                    return;
                }
                else if (again != "Y")
                {
                    Console.WriteLine("  Invalid choice!");
                    Console.ReadKey();
                }
            }
        }
        // BASIC CALCULATOR
        static void BasicCalculator()
        {
            while (true)
            {
                Console.Clear();
                Console.Beep(800, 200);

                Console.WriteLine();
                Header("BASIC CALCULATOR", ConsoleColor.DarkYellow);

                Console.ForegroundColor = ConsoleColor.DarkMagenta;
                Console.Write("\n  Enter first number: ");
                Console.ResetColor();
                double num1 = Convert.ToDouble(Console.ReadLine());

                Console.ForegroundColor = ConsoleColor.DarkMagenta;
                Console.Write("  Enter second number: ");
                Console.ResetColor();
                double num2 = Convert.ToDouble(Console.ReadLine());

                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.WriteLine("\n  Choose an operation:");
                Console.ResetColor();

                Console.WriteLine("   [1] Addition (+)");
                Console.WriteLine("   [2] Subtraction (-)");
                Console.WriteLine("   [3] Multiplication (*)");
                Console.WriteLine("   [4] Division (/)");

                Console.ForegroundColor = ConsoleColor.DarkYellow;
                Console.Write("\n  Enter choice: ");
                Console.ResetColor();
                string choice = Console.ReadLine();

                double result = 0;
                string symbol = "";

                if (choice == "1")
                {
                    result = num1 + num2;
                    symbol = "+";
                }
                else if (choice == "2")
                {
                    result = num1 - num2;
                    symbol = "-";
                }
                else if (choice == "3")
                {
                    result = num1 * num2;
                    symbol = "*";
                }
                else if (choice == "4")
                {
                    symbol = "/";

                    if (num2 == 0)
                    {
                        Console.Beep(400, 500);

                        Console.ForegroundColor = ConsoleColor.DarkYellow;
                        Console.WriteLine("  Cannot divide by zero!");
                        Console.ResetColor();

                        Console.ReadKey();
                        continue;
                    }

                    result = num1 / num2;
                }
                else
                {
                    Console.Beep(400, 500);

                    Console.ForegroundColor = ConsoleColor.DarkYellow;
                    Console.WriteLine("  Invalid choice!");
                    Console.ResetColor();

                    Console.ReadKey();
                    continue;
                }

                Console.Beep(700, 150);
                Console.Beep(900, 150);

                Console.ForegroundColor = ConsoleColor.DarkMagenta;
                Console.WriteLine("\n  .=========== RESULT ===========.");
                Console.WriteLine("   " + num1 + " " + symbol + " " + num2
                    + " = " + result.ToString("F2"));

                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.WriteLine("  '=============================' ");
                Console.WriteLine("   Calculation complete!");
                Console.ResetColor();
                W("   " + new string('#', 29) + "\n", ConsoleColor.DarkGray);

                Console.Write("\n  Calculate again? (Y/N): ");
                string again = Console.ReadLine().ToUpper();

                if (again == "N")
                {
                    return;
                }
                else if (again != "Y")
                {
                    Console.WriteLine("  Invalid choice!");
                    Console.ReadKey();
                }
            }
        }
        // INTERMEDIATE MENU
        static void IntermediateMenu()
        {
            while (true)
            {
                Console.Clear();

                Console.WriteLine();
                Header("INTERMEDIATE CATEGORY", ConsoleColor.Cyan);

                Console.WriteLine();
                Button("[1]  GRADE CALCULATOR", ConsoleColor.Cyan);
                Button("[2]  SIMPLE STOPWATCH", ConsoleColor.Cyan);
                Button("[0]  BACK TO MAIN MENU", ConsoleColor.Red);

                Prompt();
                string choice = Console.ReadLine();

                if (choice == "1")
                {
                    GRADE();
                }
                else if (choice == "2")
                {
                    SimpleStopwatch();
                }
                else if (choice == "0")
                {
                    return;
                }
                else
                {
                    W("\n  !! Invalid selection!\n", ConsoleColor.Red);
                    Console.ReadKey();
                }
            }
        }
        // GRADE COMPUTATION

        static void GRADE()
        {

            double gwa;
            double g1, g2, g3, g4, g5, g6, g7;

            Console.Clear();

            Console.WriteLine();
            Header("COMPUTE YOUR GRADE 1.00-5.00", ConsoleColor.Yellow);
            Console.WriteLine();

            Console.Write("  Enter grade 1: ");
            g1 = Convert.ToDouble(Console.ReadLine());

            Console.Write("  Enter grade 2: ");
            g2 = Convert.ToDouble(Console.ReadLine());

            Console.Write("  Enter grade 3: ");
            g3 = Convert.ToDouble(Console.ReadLine());

            Console.Write("  Enter grade 4: ");
            g4 = Convert.ToDouble(Console.ReadLine());

            Console.Write("  Enter grade 5: ");
            g5 = Convert.ToDouble(Console.ReadLine());

            Console.Write("  Enter grade 6: ");
            g6 = Convert.ToDouble(Console.ReadLine());

            Console.Write("  Enter grade 7: ");
            g7 = Convert.ToDouble(Console.ReadLine());

            gwa = (g1 + g2 + g3 + g4 + g5 + g6 + g7) / 7;
            gwa = Math.Truncate(gwa * 100) / 100;

            W("\n  Your GWA is: " + gwa + "\n", ConsoleColor.Cyan);

            if (gwa >= 1.00 && gwa <= 1.19)
                W("  *** SUMMA CUM LAUDE ***\n", ConsoleColor.Yellow);
            else if (gwa >= 1.20 && gwa <= 1.49)
                W("  ** MAGNA CUM LAUDE **\n", ConsoleColor.Green);
            else if (gwa >= 1.50 && gwa <= 1.69)
                W("  * CUM LAUDE *\n", ConsoleColor.Cyan);
            else
                W("  Nice Try\n", ConsoleColor.DarkGray);

            Console.Write("\n  Calculate again? (Y/N): ");
            string again = Console.ReadLine().ToUpper();

            if (again == "N")
            {
                return;
            }
            else if (again != "Y")
            {
                Console.WriteLine("  Invalid choice!");
                Console.ReadKey();
            }
        }

        //SIMPLE STOPWATCH


        static void SimpleStopwatch()
        {
            Console.Clear();

            Stopwatch timer = new Stopwatch();

            bool quit = false;
            bool started = false;
            bool paused = false;

            Console.WriteLine();
            Header("MY STOPWATCH", ConsoleColor.Cyan);
            Console.WriteLine("   [S] Start");
            Console.WriteLine("   [P] Pause");
            Console.WriteLine("   [R] Resume");
            Console.WriteLine("   [X] Stop and Reset");
            Console.WriteLine("   [Q] Quit");
            W("  " + new string('=', W_BOX) + "\n", ConsoleColor.DarkGray);

            int row = Console.CursorTop + 1;

            while (quit == false)
            {
                Console.SetCursorPosition(0, row);
                Console.WriteLine("  Elapsed Time: " +
                    (int)timer.Elapsed.TotalHours + ":" +
                    timer.Elapsed.Minutes.ToString("00") + ":" +
                    timer.Elapsed.Seconds.ToString("00") + "." +
                    timer.Elapsed.Milliseconds.ToString("000"));

                Console.WriteLine("  Status: " +
                    (started == false ? "READY" :
                    paused == true ? "PAUSED" : "RUNNING") + "       ");

                if (Console.KeyAvailable)
                {
                    ConsoleKey key = Console.ReadKey(true).Key;

                    switch (key)
                    {
                        case ConsoleKey.S:

                            if (started == false)
                            {
                                timer.Start();
                                started = true;
                                paused = false;
                                Console.WriteLine("  Stopwatch started.");
                            }
                            else
                            {
                                Console.WriteLine("  Already started.");
                            }

                            break;

                        case ConsoleKey.P:

                            if (started == true && paused == false)
                            {
                                timer.Stop();
                                paused = true;
                                Console.WriteLine("  Stopwatch paused.");
                            }
                            else
                            {
                                Console.WriteLine("  Cannot pause.");
                            }

                            break;

                        case ConsoleKey.R:

                            if (started == true && paused == true)
                            {
                                timer.Start();
                                paused = false;
                                Console.WriteLine("  Stopwatch resumed.");
                            }
                            else
                            {
                                Console.WriteLine("  Cannot resume.");
                            }

                            break;

                        case ConsoleKey.X:

                            timer.Reset();
                            started = false;
                            paused = false;
                            Console.WriteLine("  Stopwatch reset.");
                            break;

                        case ConsoleKey.Q:

                            quit = true;
                            break;

                        default:

                            Console.WriteLine("  Invalid key.");
                            break;
                    }
                }

                Thread.Sleep(50);
            }

            Console.WriteLine("  Goodbye!");

        }

        static void EntertainmentMenu()
        {
            while (true)
            {
                Console.Clear();

                Console.WriteLine();
                Header("ENTERTAINMENT CATEGORY", ConsoleColor.Magenta);

                Console.WriteLine();
                Button("[1]  HEAD OR TAILS", ConsoleColor.Magenta);
                Button("[2]  GUESS THE NUMBER", ConsoleColor.Magenta);
                Button("[3]  PET SIMULATOR", ConsoleColor.Magenta);
                Button("[0]  BACK TO MAIN MENU", ConsoleColor.Red);

                Prompt();
                string choice = Console.ReadLine();

                if (choice == "1")
                {
                    HeadorTails();
                }
                else if (choice == "2")
                {
                    GuesstheNumber();
                }
                else if (choice == "3")
                {
                    PetSimulator();
                }
                else if (choice == "0")
                {
                    return;
                }
                else
                {
                    W("\n  !! Invalid selection!\n", ConsoleColor.Red);
                    Console.ReadKey();
                }
            }
        }

        //HEADS OR TAILS
        static void HeadorTails()
        {

            int score = 0;
            int rounds = 5;

            Random random = new Random();

            Console.Title = "HEADS OR TAILS";
            Console.Clear();

            Console.WriteLine();
            Header("HEADS OR TAILS", ConsoleColor.Yellow);

            Console.WriteLine();
            Console.WriteLine("  Guess the result of the coin flip!");
            Console.WriteLine("  You have 5 rounds.");
            Console.WriteLine();

            Console.WriteLine("  Press ENTER to start...");
            Console.ReadLine();

            while (rounds > 0)
            {
                Console.Clear();

                Console.WriteLine();
                Header("HEADS OR TAILS", ConsoleColor.Yellow);

                Console.WriteLine();
                W("  Score: " + score + "\n", ConsoleColor.Cyan);
                W("  Rounds left: " + rounds + "\n", ConsoleColor.Cyan);

                Console.WriteLine();
                Console.WriteLine("   (1) Heads");
                Console.WriteLine("   (2) Tails");
                Console.WriteLine();

                Console.Write("  Your choice: ");
                string choice = Console.ReadLine();

                switch (choice)
                {
                    case "1":
                    case "2":

                        int coin = random.Next(1, 3);

                        Console.WriteLine();
                        Console.WriteLine("  Flipping the coin...");
                        Console.Beep(800, 150);
                        Console.Beep(1000, 150);

                        if (coin == 1)
                        {
                            Console.WriteLine("  Result: HEADS!");
                        }
                        else
                        {
                            Console.WriteLine("  Result: TAILS!");
                        }

                        if ((choice == "1" && coin == 1) ||
                            (choice == "2" && coin == 2))
                        {
                            Console.ForegroundColor = ConsoleColor.Green;
                            Console.WriteLine("  CORRECT! You get 1 point!");
                            Console.ResetColor();

                            score++;
                        }
                        else
                        {
                            Console.ForegroundColor = ConsoleColor.Red;
                            Console.WriteLine("  WRONG!");
                            Console.ResetColor();
                        }

                        rounds--;

                        Console.WriteLine();
                        Console.WriteLine("  Press ENTER for the next round...");
                        Console.ReadLine();

                        break;

                    default:

                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine();
                        Console.WriteLine("  Invalid choice! Choose 1 or 2.");
                        Console.ResetColor();

                        Console.ReadLine();

                        break;
                }
            }

            Console.Clear();

            Console.WriteLine();
            Header("GAME OVER", ConsoleColor.Yellow);

            Console.WriteLine();
            Console.WriteLine("  Final Score: " + score + "/5");

            if (score >= 3)
            {
                Console.ForegroundColor = ConsoleColor.Green;
                Console.WriteLine("  Great job! You won!");
                Console.ResetColor();
            }
            else
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("  Better luck next time!");
                Console.ResetColor();
            }

            Console.Write("\n  Calculate again? (Y/N): ");
            string again = Console.ReadLine().ToUpper();

            if (again == "N")
            {
                return;
            }
            else if (again != "Y")
            {
                Console.WriteLine("  Invalid choice!");
                Console.ReadKey();
            }

            //GUESS THE NUMBER
        }
        static void GuesstheNumber()
        {
            int chances = 3;
            int points = 0;
            bool gameOver = false;

            Random random = new Random();

            Console.Title = "LUCKY NUMBER";
            Console.Clear();

            Console.WriteLine();
            Header("LUCKY NUMBER", ConsoleColor.Green);

            Console.WriteLine();
            Console.WriteLine("  The computer will choose a number");
            Console.WriteLine("  from 1 to 10.");
            Console.WriteLine();
            Console.WriteLine("  You have 3 chances to guess it!");
            Console.WriteLine();

            Console.WriteLine("  Press ENTER to start...");
            Console.ReadLine();

            while (!gameOver)
            {
                Console.Clear();

                Console.WriteLine();
                Header("LUCKY NUMBER", ConsoleColor.Green);

                Console.WriteLine();
                W("  Chances left: " + chances + "\n", ConsoleColor.Cyan);
                W("  Points: " + points + "\n", ConsoleColor.Cyan);
                Console.WriteLine();

                Console.Write("  Pick a number from 1 to 10: ");
                string input = Console.ReadLine();

                int guess;

                if (int.TryParse(input, out guess))
                {
                    if (guess >= 1 && guess <= 10)
                    {
                        int luckyNumber = random.Next(1, 11);

                        Console.WriteLine();
                        Console.WriteLine("  The lucky number was: " + luckyNumber);

                        if (guess == luckyNumber)
                        {
                            Console.ForegroundColor = ConsoleColor.Green;
                            Console.WriteLine();
                            Console.WriteLine("  🎉 JACKPOT! YOU GUESSED IT!");
                            Console.ResetColor();

                            points++;
                        }
                        else
                        {
                            Console.ForegroundColor = ConsoleColor.Red;
                            Console.WriteLine();
                            Console.WriteLine("  Wrong guess!");
                            Console.ResetColor();
                        }

                        chances--;

                        if (points == 1)
                        {
                            Console.WriteLine();
                            Console.WriteLine("  You got 1 correct guess!");
                        }

                        if (chances == 0)
                        {
                            Console.WriteLine();

                            if (points > 0)
                            {
                                Console.ForegroundColor = ConsoleColor.Green;
                                Console.WriteLine("  You win!");
                                Console.WriteLine("  Final score: " + points);
                                Console.ResetColor();
                            }
                            else
                            {
                                Console.ForegroundColor = ConsoleColor.Red;
                                Console.WriteLine("  Game over!");
                                Console.WriteLine("  You didn't guess the lucky number.");
                                Console.ResetColor();
                            }

                            gameOver = true;
                        }
                        else
                        {
                            Console.WriteLine();
                            Console.WriteLine("  Press ENTER to try again...");
                            Console.ReadLine();
                        }
                    }
                    else
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine();
                        Console.WriteLine("  Please enter a number from 1 to 10.");
                        Console.ResetColor();

                        Console.ReadLine();
                    }
                }
                else
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine();
                    Console.WriteLine("  That's not a valid number!");
                    Console.ResetColor();

                    Console.ReadLine();
                }
            }

            Console.WriteLine();
            Console.WriteLine("  Thanks for playing!");

            Console.Write("\n  Calculate again? (Y/N): ");
            string again = Console.ReadLine().ToUpper();

            if (again == "N")
            {
                return;
            }
            else if (again != "Y")
            {
                Console.WriteLine("  Invalid choice!");
                Console.ReadKey();
            }
        }
        //PET SIMULATOR

        static void PetSimulator()
        {
            Console.Clear();
            Console.Title = "Virtual Pet Simulator";

            bool again = true;

            while (again == true)
            {
                Console.Clear();

                Console.WriteLine();
                Header("VIRTUAL PET SIMULATOR", ConsoleColor.Cyan);

                Console.Write("\n  Enter your pet's name: ");
                string petName = Console.ReadLine();

                Console.WriteLine("  Choose your pet:");
                Console.WriteLine("   1. Cat");
                Console.WriteLine("   2. Dog");
                Console.WriteLine("   3. Rabbit");

                Console.Write("  Enter Choice: ");
                int petChoice = Convert.ToInt32(Console.ReadLine());

                string petType = "";

                if (petChoice == 1)
                {
                    petType = "Cat";
                }
                else if (petChoice == 2)
                {
                    petType = "Dog";
                }
                else if (petChoice == 3)
                {
                    petType = "Rabbit";
                }
                else
                {
                    Console.WriteLine("  Invalid Selection!");
                    Console.ReadKey();
                    continue;
                }

                int hunger = 50;
                int happiness = 50;
                int energy = 50;

                bool playing = true;

                while (playing == true)
                {
                    Console.Clear();

                    Console.WriteLine();
                    Header("MY VIRTUAL PET", ConsoleColor.Yellow);

                    Console.WriteLine("\n  Pet Name: " + petName);
                    Console.WriteLine("  Pet Type: " + petType);

                    W("\n  .========== PET STATUS ==========.\n", ConsoleColor.Cyan);

                    Console.WriteLine("   Hunger:    " + hunger + "/100");
                    Console.WriteLine("   Happiness: " + happiness + "/100");
                    Console.WriteLine("   Energy:    " + energy + "/100");

                    W("  '================================'", ConsoleColor.Cyan);
                    W("#\n", ConsoleColor.DarkGray);
                    W("   " + new string('#', 32) + "\n", ConsoleColor.DarkGray);

                    W("\n  .=========== PET MENU ===========.\n", ConsoleColor.Magenta);

                    Console.WriteLine("   1. Feed Your Pet");
                    Console.WriteLine("   2. Play With Your Pet");
                    Console.WriteLine("   3. Let Your Pet Sleep");
                    Console.WriteLine("   4. Check Pet Status");
                    Console.WriteLine("   5. Exit");

                    W("  '================================'", ConsoleColor.Magenta);
                    W("#\n", ConsoleColor.DarkGray);
                    W("   " + new string('#', 32) + "\n", ConsoleColor.DarkGray);

                    Console.Write("\n  Enter Choice: ");
                    int choice = Convert.ToInt32(Console.ReadLine());

                    if (choice == 1)
                    {
                        hunger = hunger - 20;

                        if (hunger < 0)
                        {
                            hunger = 0;
                        }

                        happiness = happiness + 10;

                        if (happiness > 100)
                        {
                            happiness = 100;
                        }

                        Console.ForegroundColor = ConsoleColor.Green;
                        Console.WriteLine("\n  " + petName + " enjoyed the food!");
                        Console.ResetColor();
                    }
                    else if (choice == 2)
                    {
                        if (energy >= 20)
                        {
                            happiness = happiness + 20;
                            energy = energy - 20;
                            hunger = hunger + 10;

                            if (happiness > 100)
                            {
                                happiness = 100;
                            }

                            if (hunger > 100)
                            {
                                hunger = 100;
                            }

                            Console.ForegroundColor = ConsoleColor.Green;
                            Console.WriteLine("\n  " + petName + " had fun playing!");
                            Console.ResetColor();
                        }
                        else
                        {
                            Console.WriteLine("\n  " + petName + " is too tired to play!");
                        }
                    }
                    else if (choice == 3)
                    {
                        energy = energy + 30;

                        if (energy > 100)
                        {
                            energy = 100;
                        }

                        Console.WriteLine("\n  " + petName + " is sleeping...");
                    }
                    else if (choice == 4)
                    {
                        W("\n  .========== PET STATUS ==========.\n", ConsoleColor.Cyan);

                        Console.WriteLine("   Name: " + petName);
                        Console.WriteLine("   Type: " + petType);
                        Console.WriteLine("   Hunger: " + hunger);
                        Console.WriteLine("   Happiness: " + happiness);
                        Console.WriteLine("   Energy: " + energy);
                    }
                    else if (choice == 5)
                    {
                        playing = false;
                    }
                    else
                    {
                        Console.WriteLine("\n  Invalid Selection!");
                    }

                    if (playing == true)
                    {
                        Console.WriteLine("\n  Press any key to continue...");
                        Console.ReadKey();
                    }
                }

                Console.Clear();

                Console.WriteLine();
                Header("THANK YOU FOR PLAYING!", ConsoleColor.Cyan);

                Console.WriteLine("\n  Do you want to play again?");
                Console.WriteLine("   1. Try Again");
                Console.WriteLine("   2. Quit");

                Console.Write("\n  Enter Choice: ");
                int answer = Convert.ToInt32(Console.ReadLine());

                if (answer == 1)
                {
                    again = true;
                }
                else if (answer == 2)
                {
                    again = false;
                }
                else
                {
                    Console.WriteLine("  Invalid Selection!");
                    Console.ReadKey();
                    again = true;
                }
            }

            Console.Clear();

            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.WriteLine("\n  Thank you for using Virtual Pet Simulator!");
            Console.ResetColor();

            Console.ReadKey();
        }

        //EXIT MENU
        static void ExitProgram()
        {
            Console.Clear();

            Console.WriteLine();
            Logo();
            Header("HYDRA CONSOLE", ConsoleColor.Cyan);

            Console.WriteLine();
            Console.WriteLine("            THANK YOU FOR PLAYING!");
            Console.WriteLine();
            Console.WriteLine("       Thank you for using HYDRA CONSOLE.");
            Console.WriteLine("            We hope you enjoyed!");
            Console.WriteLine();
            W("                  GOODBYE!\n", ConsoleColor.Yellow);
            Console.WriteLine();
            W("  " + new string('=', W_BOX) + "\n", ConsoleColor.DarkGray);

            Console.ReadKey();
        }
    }
}
