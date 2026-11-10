
using System;
using System.Diagnostics;
using System.Threading;

namespace HydraConsole
{
    internal class Program
    {
        // ============================================================
        // HYDRA SETTINGS
        // ============================================================

        const int W_BOX = 52;
        const double ELECTRICITY_RATE = 12.00;

        static Random random = new Random();

        // ============================================================
        // ASCII DESIGN HELPERS
        // ============================================================

        static void W(string text, ConsoleColor color)
        {
            Console.ForegroundColor = color;
            Console.Write(text);
            Console.ResetColor();
        }

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

            ConsoleColor[] colors =
            {
                ConsoleColor.White,
                ConsoleColor.Cyan,
                ConsoleColor.Cyan,
                ConsoleColor.DarkCyan,
                ConsoleColor.DarkCyan
            };

            for (int i = 0; i < logo.Length; i++)
            {
                W("       " + logo[i] + "\n", colors[i]);
            }

            W("        " + new string('#', 37) + "\n",
                ConsoleColor.DarkGray);

            W("         " + new string('.', 35) + "\n",
                ConsoleColor.DarkGray);
        }

        static void Header(string title, ConsoleColor color)
        {
            int width = W_BOX - 2;

            if (title.Length > width)
            {
                title = title.Substring(0, width);
            }

            int left = (width - title.Length) / 2;
            int right = width - title.Length - left;

            string middle =
                new string(' ', left) +
                title +
                new string(' ', right);

            string bar = new string('=', width);

            W(" ." + bar + ".\n", color);

            W(" |" + middle + "|", color);
            W("#\n", ConsoleColor.DarkGray);

            W(" '" + bar + "'", color);
            W("#\n", ConsoleColor.DarkGray);

            W("  " + new string('#', W_BOX) + "\n",
                ConsoleColor.DarkGray);
        }

        static void Button(string text, ConsoleColor color)
        {
            int width = 36;

            string label = " " + text;

            if (label.Length > width - 2)
            {
                label = label.Substring(0, width - 2);
            }

            string middle = label.PadRight(width - 2);
            string bar = new string('-', width - 2);

            W("      +" + bar + "+\n", color);

            W("      |" + middle + "|", color);
            W("#\n", ConsoleColor.DarkGray);

            W("      +" + bar + "+", color);
            W("#\n", ConsoleColor.DarkGray);
        }

        static void Prompt()
        {
            W("\n  >> ", ConsoleColor.DarkGray);
            Console.Write("Enter your choice: ");
        }

        static void ShowError(string message)
        {
            W("\n  !! " + message + "\n", ConsoleColor.Red);
        }

        static void Pause()
        {
            Console.Write("\n  Press any key to continue...");
            Console.ReadKey(true);
        }

        // ============================================================
        // INPUT VALIDATION
        // ============================================================

        static string ReadText(string prompt)
        {
            Console.Write(prompt);

            string input = Console.ReadLine();

            if (input == null)
            {
                return "";
            }

            return input.Trim();
        }

        static int ReadInt(string prompt, int minimum, int maximum)
        {
            int value;

            while (true)
            {
                Console.Write(prompt);

                string input = Console.ReadLine();

                if (int.TryParse(input, out value))
                {
                    if (value >= minimum && value <= maximum)
                    {
                        return value;
                    }
                }

                ShowError("Enter a valid whole number from "
                    + minimum + " to " + maximum + ".");
            }
        }

        static double ReadDouble(string prompt, double minimum, double maximum)
        {
            double value;

            while (true)
            {
                Console.Write(prompt);

                string input = Console.ReadLine();

                if (double.TryParse(input, out value))
                {
                    if (!double.IsNaN(value) &&
                        !double.IsInfinity(value) &&
                        value >= minimum &&
                        value <= maximum)
                    {
                        return value;
                    }
                }

                ShowError("Enter a valid number within the allowed range.");
            }
        }

        static bool AskAgain(string prompt)
        {
            while (true)
            {
                string answer = ReadText(prompt).ToUpper();

                if (answer == "Y")
                {
                    return true;
                }

                if (answer == "N")
                {
                    return false;
                }

                ShowError("Please enter Y or N.");
            }
        }

        // ============================================================
        // MAIN PROGRAM
        // ============================================================

        static void Main(string[] args)
        {
            Console.Title = "HYDRA CONSOLE";
            Console.BackgroundColor = ConsoleColor.Black;
            Console.ForegroundColor = ConsoleColor.Green;
            Console.Clear();

            WelcomePage();

            if (LoginPage() == false)
            {
                return;
            }

            LoadingAnimation();

            MainMenu();

            ExitProgram();
        }

        // ============================================================
        // WELCOME PAGE
        // ============================================================

        static void WelcomePage()
        {
            Console.Clear();

            Console.WriteLine();
            Logo();

            Header("HYDRA CONSOLE", ConsoleColor.Green);

            Console.WriteLine();

            W("  Welcome to HYDRA Console!\n",
                ConsoleColor.Green);

            Console.WriteLine("  Your personal console utility system.");

            Pause();
        }

        // ============================================================
        // LOGIN PAGE
        // ============================================================

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

                W("\n  [USER] ", ConsoleColor.Cyan);

                string username = ReadText("Enter Username: ");

                W("  [PASS] ", ConsoleColor.Cyan);

                string password = ReadText("Enter Password: ");

                if (username == "user" && password == "pass")
                {
                    W("\n  >> LOGIN SUCCESSFUL!\n",
                        ConsoleColor.Green);

                    Pause();

                    return true;
                }

                attempts++;

                ShowError("Invalid username or password.");

                Console.WriteLine(
                    "  Attempts remaining: " +
                    (maxAttempts - attempts));

                Pause();
            }

            Console.Clear();

            Header("ACCESS DENIED", ConsoleColor.Red);

            ShowError("Maximum login attempts reached.");

            W("  ACCESS DENIED.\n", ConsoleColor.Red);

            Pause();

            return false;
        }

        // ============================================================
        // LOADING ANIMATION
        // ============================================================

        static void LoadingAnimation()
        {
            Console.Clear();

            Console.WriteLine();
            Logo();

            Header("SYSTEM INITIALIZATION", ConsoleColor.Green);

            W("\n  AUTHENTICATION SUCCESSFUL!\n",
                ConsoleColor.Green);

            Console.WriteLine("  Initializing HYDRA SYSTEM...\n");

            Console.Write("  Loading: ");

            W("[", ConsoleColor.DarkGray);

            for (int i = 0; i < 20; i++)
            {
                ConsoleColor color;

                if (i < 7)
                {
                    color = ConsoleColor.DarkCyan;
                }
                else if (i < 14)
                {
                    color = ConsoleColor.Cyan;
                }
                else
                {
                    color = ConsoleColor.Green;
                }

                W("#", color);

                Thread.Sleep(70);
            }

            W("]", ConsoleColor.DarkGray);

            W("\n\n  >> SYSTEM READY!\n",
                ConsoleColor.Green);

            Thread.Sleep(500);
        }

        // ============================================================
        // MAIN MENU
        // ============================================================

        static void MainMenu()
        {
            while (true)
            {
                Console.Clear();

                Console.WriteLine();
                Logo();

                Header("CONSOLE SYSTEM // MAIN MENU",
                    ConsoleColor.Green);

                Console.WriteLine();

                Button("[1] BASIC", ConsoleColor.Cyan);
                Button("[2] INTERMEDIATE", ConsoleColor.Yellow);
                Button("[3] ENTERTAINMENT", ConsoleColor.Magenta);
                Button("[0] EXIT", ConsoleColor.Red);

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
                else if (choice == "0" || choice == null)
                {
                    return;
                }
                else
                {
                    ShowError("Invalid selection.");
                    Pause();
                }
            }
        }

        // ============================================================
        // BASIC CATEGORY
        // ============================================================

        static void BasicMenu()
        {
            while (true)
            {
                Console.Clear();

                Console.WriteLine();

                Logo();
                Header("BASIC CATEGORY", ConsoleColor.Green);

                Console.WriteLine();

                Button("[1] BILL CALCULATOR", ConsoleColor.Cyan);
                Button("[2] BASIC CALCULATOR", ConsoleColor.Cyan);
                Button("[0] BACK TO MAIN MENU", ConsoleColor.Red);

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
                else if (choice == "0" || choice == null)
                {
                    return;
                }
                else
                {
                    ShowError("Invalid selection.");
                    Pause();
                }
            }
        }

        // ============================================================
        // ELECTRICITY BILL CALCULATOR
        // ============================================================

        static void ElectricityBillCalculator()
        {
            while (true)
            {
                Console.Clear();

                Console.WriteLine();

                Logo();
                Header("ELECTRICITY BILL CALCULATOR",
                    ConsoleColor.Yellow);

                string name = ReadText(
                    "\n  Enter Customer Name: ");

                if (name == "")
                {
                    name = "Customer";
                }

                double kwh = ReadDouble(
                    "  Enter Electricity Consumption (kWh): ",
                    0,
                    double.MaxValue);

                double bill = kwh * ELECTRICITY_RATE;

                W("\n  .=========== BILL SUMMARY ===========.\n",
                    ConsoleColor.Cyan);

                Console.WriteLine("   Customer Name: " + name);

                Console.WriteLine(
                    "   Consumption: " + kwh.ToString("F2") + " kWh");

                Console.WriteLine(
                    "   Rate per kWh: PHP " +
                    ELECTRICITY_RATE.ToString("F2"));

                W("   Total Bill: PHP " +
                    bill.ToString("F2") + "\n",
                    ConsoleColor.Green);

                if (bill >= 2000)
                {
                    W("   Reminder: High electricity bill!\n",
                        ConsoleColor.Red);
                }
                else
                {
                    W("   Reminder: Thank you for using our service!\n",
                        ConsoleColor.Green);
                }

                W("  '====================================='\n",
                    ConsoleColor.Cyan);

                W("   " + new string('#', 37) + "\n",
                    ConsoleColor.DarkGray);

                if (AskAgain("\n  Calculate another bill? (Y/N): ") == false)
                {
                    return;
                }
            }
        }

        // ============================================================
        // BASIC CALCULATOR
        // ============================================================

        static void BasicCalculator()
        {
            while (true)
            {
                Console.Clear();

                Console.WriteLine();

                Logo();
                Header("BASIC CALCULATOR",
                    ConsoleColor.DarkYellow);

                double num1 = ReadDouble(
                    "\n  Enter first number: ",
                    -double.MaxValue,
                    double.MaxValue);

                double num2 = ReadDouble(
                    "  Enter second number: ",
                    -double.MaxValue,
                    double.MaxValue);

                Console.WriteLine("\n  Choose an operation:");

                Console.WriteLine("   [1] Addition (+)");
                Console.WriteLine("   [2] Subtraction (-)");
                Console.WriteLine("   [3] Multiplication (*)");
                Console.WriteLine("   [4] Division (/)");

                string choice = ReadText("\n  Enter choice: ");

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
                    if (num2 == 0)
                    {
                        ShowError("Cannot divide by zero.");
                        Pause();
                        continue;
                    }

                    result = num1 / num2;
                    symbol = "/";
                }
                else
                {
                    ShowError("Invalid operation.");
                    Pause();
                    continue;
                }

                W("\n  .=========== RESULT ===========.\n",
                    ConsoleColor.Cyan);

                Console.WriteLine(
                    "   " + num1 + " " + symbol + " " +
                    num2 + " = " + result.ToString("F2"));

                W("  '==============================='\n",
                    ConsoleColor.Cyan);

                Console.WriteLine("   Calculation complete!");

                if (AskAgain("\n  Calculate again? (Y/N): ") == false)
                {
                    return;
                }
            }
        }

        // ============================================================
        // INTERMEDIATE CATEGORY
        // ============================================================

        static void IntermediateMenu()
        {
            while (true)
            {
                Console.Clear();

                Console.WriteLine();

                Logo();
                Header("INTERMEDIATE CATEGORY",
                    ConsoleColor.Cyan);

                Console.WriteLine();

                Button("[1] GRADE CALCULATOR", ConsoleColor.Cyan);
                Button("[2] SIMPLE STOPWATCH", ConsoleColor.Cyan);
                Button("[0] BACK TO MAIN MENU", ConsoleColor.Red);

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
                else if (choice == "0" || choice == null)
                {
                    return;
                }
                else
                {
                    ShowError("Invalid selection.");
                    Pause();
                }
            }
        }

        // ============================================================
        // GRADE COMPUTATION
        // ============================================================

        static void GRADE()
        {
            while (true)
            {
                Console.Clear();

                Console.WriteLine();

                Logo();
                Header("COMPUTE YOUR GRADE 1.00 - 5.00",
                    ConsoleColor.Yellow);

                Console.WriteLine();

                double total = 0;

                for (int i = 1; i <= 7; i++)
                {
                    total += ReadDouble(
                        "  Enter grade " + i + ": ",
                        1.00,
                        5.00);
                }

                double gwa = total / 7.0;

                gwa = Math.Truncate(gwa * 100) / 100;

                W("\n  Your GWA is: " + gwa.ToString("F2") + "\n",
                    ConsoleColor.Cyan);

                if (gwa >= 1.00 && gwa <= 1.19)
                {
                    W("  *** SUMMA CUM LAUDE ***\n",
                        ConsoleColor.Yellow);
                }
                else if (gwa >= 1.20 && gwa <= 1.49)
                {
                    W("  ** MAGNA CUM LAUDE **\n",
                        ConsoleColor.Green);
                }
                else if (gwa >= 1.50 && gwa <= 1.69)
                {
                    W("  * CUM LAUDE *\n",
                        ConsoleColor.Cyan);
                }
                else
                {
                    W("  Nice Try\n", ConsoleColor.DarkGray);
                }

                if (AskAgain("\n  Calculate again? (Y/N): ") == false)
                {
                    return;
                }
            }
        }

        // ============================================================
        // SIMPLE STOPWATCH
        // ============================================================

        static void SimpleStopwatch()
        {
            Console.Clear();

            Stopwatch timer = new Stopwatch();

            bool quit = false;
            bool started = false;
            bool paused = false;

            Console.WriteLine();

            Logo();
            Header("MY STOPWATCH", ConsoleColor.Cyan);

            Console.WriteLine("   [S] Start");
            Console.WriteLine("   [P] Pause");
            Console.WriteLine("   [R] Resume");
            Console.WriteLine("   [X] Stop and Reset");
            Console.WriteLine("   [Q] Quit");

            W("  " + new string('=', W_BOX) + "\n",
                ConsoleColor.DarkGray);

            int row = Console.CursorTop;

            Console.WriteLine();
            Console.WriteLine("  Elapsed Time: 00:00:00.000");
            Console.WriteLine("  Status: READY");
            Console.WriteLine();
            Console.WriteLine("  Use the keys listed above.");

            int messageRow = row + 6;

            while (!quit)
            {
                try
                {
                    Console.SetCursorPosition(2, row + 1);

                    Console.Write(
                        "Elapsed Time: " +
                        (int)timer.Elapsed.TotalHours + ":" +
                        timer.Elapsed.Minutes.ToString("00") + ":" +
                        timer.Elapsed.Seconds.ToString("00") + "." +
                        timer.Elapsed.Milliseconds.ToString("000") +
                        "       ");

                    Console.SetCursorPosition(2, row + 2);

                    string status;

                    if (!started)
                    {
                        status = "READY";
                    }
                    else if (paused)
                    {
                        status = "PAUSED";
                    }
                    else
                    {
                        status = "RUNNING";
                    }

                    Console.Write("Status: " + status + "       ");

                    Console.SetCursorPosition(0, messageRow);
                }
                catch (ArgumentOutOfRangeException)
                {
                    // Ignore display refresh errors if the window is too small.
                }

                if (Console.KeyAvailable)
                {
                    ConsoleKey key = Console.ReadKey(true).Key;
                    string message = "";

                    switch (key)
                    {
                        case ConsoleKey.S:

                            if (!started)
                            {
                                timer.Start();
                                started = true;
                                paused = false;
                                message = "Stopwatch started.";
                            }
                            else
                            {
                                message = "Stopwatch is already started.";
                            }

                            break;

                        case ConsoleKey.P:

                            if (started && !paused)
                            {
                                timer.Stop();
                                paused = true;
                                message = "Stopwatch paused.";
                            }
                            else
                            {
                                message = "Cannot pause right now.";
                            }

                            break;

                        case ConsoleKey.R:

                            if (started && paused)
                            {
                                timer.Start();
                                paused = false;
                                message = "Stopwatch resumed.";
                            }
                            else
                            {
                                message = "Cannot resume right now.";
                            }

                            break;

                        case ConsoleKey.X:

                            timer.Reset();
                            started = false;
                            paused = false;
                            message = "Stopwatch reset.";

                            break;

                        case ConsoleKey.Q:

                            quit = true;
                            message = "Closing stopwatch...";

                            break;

                        default:

                            message = "Invalid key.";

                            break;
                    }

                    try
                    {
                        Console.SetCursorPosition(2, messageRow);
                        Console.Write(message.PadRight(50));
                    }
                    catch (ArgumentOutOfRangeException)
                    {
                        Console.WriteLine(message);
                    }
                }

                Thread.Sleep(50);
            }

            Console.WriteLine("\n\n  Stopwatch closed.");

            Pause();
        }

        // ============================================================
        // ENTERTAINMENT CATEGORY
        // ============================================================

        static void EntertainmentMenu()
        {
            while (true)
            {
                Console.Clear();

                Console.WriteLine();

                Logo();
                Header("ENTERTAINMENT CATEGORY",
                    ConsoleColor.Magenta);

                Console.WriteLine();

                Button("[1] HEADS OR TAILS", ConsoleColor.Magenta);
                Button("[2] GUESS THE NUMBER", ConsoleColor.Magenta);
                Button("[3] PET SIMULATOR", ConsoleColor.Magenta);
                Button("[0] BACK TO MAIN MENU", ConsoleColor.Red);

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
                else if (choice == "0" || choice == null)
                {
                    return;
                }
                else
                {
                    ShowError("Invalid selection.");
                    Pause();
                }
            }
        }

        // ============================================================
        // HEADS OR TAILS
        // ============================================================

        static void HeadorTails()
        {
            while (true)
            {
                int score = 0;
                int rounds = 5;

                Console.Title = "HEADS OR TAILS";

                Console.Clear();

                Console.WriteLine();

                Logo();
                Header("HEADS OR TAILS", ConsoleColor.Yellow);

                Console.WriteLine();

                Console.WriteLine("  Guess the result of the coin flip!");
                Console.WriteLine("  You have 5 rounds.");

                Pause();

                while (rounds > 0)
                {
                    Console.Clear();

                    Console.WriteLine();

                    Logo();
                    Header("HEADS OR TAILS", ConsoleColor.Yellow);

                    W("\n  Score: " + score + "\n",
                        ConsoleColor.Cyan);

                    W("  Rounds left: " + rounds + "\n",
                        ConsoleColor.Cyan);

                    Console.WriteLine();

                    Console.WriteLine("   [1] Heads");
                    Console.WriteLine("   [2] Tails");

                    string choice = ReadText("\n  Your choice: ");

                    if (choice != "1" && choice != "2")
                    {
                        ShowError("Choose 1 or 2.");
                        Pause();
                        continue;
                    }

                    int coin = random.Next(1, 3);

                    Console.WriteLine("\n  Flipping the coin...");

                    Thread.Sleep(500);

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
                        W("  CORRECT! You get 1 point!\n",
                            ConsoleColor.Green);

                        score++;
                    }
                    else
                    {
                        W("  WRONG!\n", ConsoleColor.Red);
                    }

                    rounds--;

                    if (rounds > 0)
                    {
                        Pause();
                    }
                }

                Console.Clear();

                Console.WriteLine();

                Logo();
                Header("GAME OVER", ConsoleColor.Yellow);

                Console.WriteLine();

                Console.WriteLine("  Final Score: " + score + "/5");

                if (score >= 3)
                {
                    W("  Great job! You won!\n",
                        ConsoleColor.Green);
                }
                else
                {
                    W("  Better luck next time!\n",
                        ConsoleColor.Red);
                }

                if (!AskAgain("\n  Play again? (Y/N): "))
                {
                    return;
                }
            }
        }

        // ============================================================
        // GUESS THE NUMBER
        // ============================================================

        static void GuesstheNumber()
        {
            while (true)
            {
                int chances = 3;

                int luckyNumber = random.Next(1, 11);

                bool won = false;

                Console.Title = "LUCKY NUMBER";

                Console.Clear();

                Console.WriteLine();

                Logo();
                Header("LUCKY NUMBER", ConsoleColor.Green);

                Console.WriteLine();

                Console.WriteLine(
                    "  The computer will choose a number from 1 to 10.");

                Console.WriteLine("  You have 3 chances to guess it!");

                Pause();

                while (chances > 0 && !won)
                {
                    Console.Clear();

                    Console.WriteLine();

                    Logo();
                    Header("LUCKY NUMBER", ConsoleColor.Green);

                    W("\n  Chances left: " + chances + "\n",
                        ConsoleColor.Cyan);

                    int guess = ReadInt(
                        "\n  Pick a number from 1 to 10: ",
                        1,
                        10);

                    if (guess == luckyNumber)
                    {
                        W("\n  JACKPOT! YOU GUESSED IT!\n",
                            ConsoleColor.Green);

                        won = true;
                    }
                    else
                    {
                        if (guess > luckyNumber)
                        {
                            W("\n  Too high!\n",
                                ConsoleColor.Red);
                        }
                        else
                        {
                            W("\n  Too low!\n",
                                ConsoleColor.Red);
                        }

                        chances--;

                        if (chances > 0)
                        {
                            Pause();
                        }
                    }
                }

                if (won)
                {
                    W("\n  YOU WIN!\n", ConsoleColor.Green);
                }
                else
                {
                    W("\n  GAME OVER!\n", ConsoleColor.Red);

                    Console.WriteLine(
                        "  The lucky number was: " + luckyNumber);
                }

                if (!AskAgain("\n  Play again? (Y/N): "))
                {
                    return;
                }
            }
        }

        // ============================================================
        // PET SIMULATOR
        // ============================================================

        static void PetSimulator()
        {
            Console.Title = "VIRTUAL PET SIMULATOR";

            bool again = true;

            while (again)
            {
                Console.Clear();

                Console.WriteLine();

                Logo();
                Header("VIRTUAL PET SIMULATOR",
                    ConsoleColor.Cyan);

                string petName = ReadText(
                    "\n  Enter your pet's name: ");

                if (petName == "")
                {
                    petName = "Buddy";
                }

                Console.WriteLine("\n  Choose your pet:");

                Console.WriteLine("   [1] Cat");
                Console.WriteLine("   [2] Dog");
                Console.WriteLine("   [3] Rabbit");

                int petChoice = ReadInt(
                    "\n  Enter choice: ",
                    1,
                    3);

                string petType;

                if (petChoice == 1)
                {
                    petType = "Cat";
                }
                else if (petChoice == 2)
                {
                    petType = "Dog";
                }
                else
                {
                    petType = "Rabbit";
                }

                int hunger = 50;
                int happiness = 50;
                int energy = 50;

                bool playing = true;

                while (playing)
                {
                    Console.Clear();

                    Console.WriteLine();

                    Logo();
                    Header("MY VIRTUAL PET",
                        ConsoleColor.Yellow);

                    Console.WriteLine("\n  Pet Name: " + petName);
                    Console.WriteLine("  Pet Type: " + petType);

                    W("\n  .========== PET STATUS ==========.\n",
                        ConsoleColor.Cyan);

                    Console.WriteLine("   Hunger:    " + hunger + "/100");
                    Console.WriteLine("   Happiness: " + happiness + "/100");
                    Console.WriteLine("   Energy:    " + energy + "/100");

                    W("  '================================'\n",
                        ConsoleColor.Cyan);

                    W("\n  .=========== PET MENU ===========.\n",
                        ConsoleColor.Magenta);

                    Console.WriteLine("   [1] Feed Your Pet");
                    Console.WriteLine("   [2] Play With Your Pet");
                    Console.WriteLine("   [3] Let Your Pet Sleep");
                    Console.WriteLine("   [4] Check Pet Status");
                    Console.WriteLine("   [0] Exit");

                    W("  '================================'\n",
                        ConsoleColor.Magenta);

                    int choice = ReadInt(
                        "\n  Enter choice: ",
                        0,
                        4);

                    if (choice == 1)
                    {
                        hunger = Math.Max(0, hunger - 20);

                        happiness = Math.Min(100, happiness + 10);

                        W("\n  " + petName +
                            " enjoyed the food!\n",
                            ConsoleColor.Green);
                    }
                    else if (choice == 2)
                    {
                        if (energy >= 20)
                        {
                            happiness = Math.Min(
                                100,
                                happiness + 20);

                            energy = Math.Max(
                                0,
                                energy - 20);

                            hunger = Math.Min(
                                100,
                                hunger + 10);

                            W("\n  " + petName +
                                " had fun playing!\n",
                                ConsoleColor.Green);
                        }
                        else
                        {
                            ShowError(
                                petName + " is too tired to play.");
                        }
                    }
                    else if (choice == 3)
                    {
                        energy = Math.Min(
                            100,
                            energy + 30);

                        Console.WriteLine(
                            "\n  " + petName + " is sleeping...");
                    }
                    else if (choice == 4)
                    {
                        W("\n  .========== PET STATUS ==========.\n",
                            ConsoleColor.Cyan);

                        Console.WriteLine("   Name: " + petName);
                        Console.WriteLine("   Type: " + petType);
                        Console.WriteLine("   Hunger: " + hunger + "/100");
                        Console.WriteLine("   Happiness: " + happiness + "/100");
                        Console.WriteLine("   Energy: " + energy + "/100");

                        W("  '================================'\n",
                            ConsoleColor.Cyan);
                    }
                    else if (choice == 0)
                    {
                        playing = false;
                    }

                    if (playing)
                    {
                        Pause();
                    }
                }

                Console.Clear();

                Console.WriteLine();

                Logo();
                Header("THANK YOU FOR PLAYING!",
                    ConsoleColor.Cyan);

                if (!AskAgain("\n  Create another pet? (Y/N): "))
                {
                    again = false;
                }
            }

            Console.Clear();

            W("\n  Thank you for using Virtual Pet Simulator!\n",
                ConsoleColor.Cyan);

            Pause();
        }

        // ============================================================
        // EXIT PROGRAM
        // ============================================================

        static void ExitProgram()
        {
            Console.Clear();

            Console.WriteLine();

            Logo();
            Header("HYDRA SYSTEM SHUTDOWN",
                ConsoleColor.Cyan);

            Console.WriteLine();

            Console.WriteLine(
                "       Thank you for using HYDRA CONSOLE.");

            Console.WriteLine(
                "              We hope you enjoyed!");

            Console.WriteLine();

            W("                  GOODBYE!\n",
                ConsoleColor.Yellow);

            W("  " + new string('=', W_BOX) + "\n",
                ConsoleColor.DarkGray);

            Pause();

            Console.ResetColor();

            Console.BackgroundColor = ConsoleColor.Black;

            Console.Clear();
        }
    }
}
