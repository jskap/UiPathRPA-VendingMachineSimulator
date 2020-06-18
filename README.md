# UiPathRPA-VendingMachineSimulator
Create a workflow that simulates a vending machine. The functionality should be the following:

 -- The user is asked to choose the initial credit ($5, $10, $20) and to choose if he/she wants to proceed or if they changed their mind and want to get the change;
 -- If they want to continue: They are asked what drink they want: Coffee ($3) or Tea ($2).

Next, the vending machine should check if the user has enough credit for selected drink. If Yes, display a success message and 
subtract the price of the drink from the total amount.

The user is then asked if he/she wants to buy another drink or to get the change.

If they want to get the change, a message box is displayed with the value of the change to be given to the customer.

In UiPath Studio, create a new project and perform the following steps:

1. Start the project as a 'Sequence' and drag a 'State Machine' inside it.

2. Create a State ("Select amount") and link it to the Start. In the 'Entry' section:

   -- Use an 'Input Dialog' activity to get the user input. Define the options in the designated field in the Properties panel: 
      {"$5", "$10", "$20"}. Store the output in a newly created variable ("ChosenAmount");
      
   -- Use an 'Assign' activity to convert the amount into Int32 using the cint() method and store it in a newly created Int32 
      variable;
      
   -- Use an 'Input Dialog' activity to ask the user if she wants to continue. It should have 2 options 
      ("Yes", "No, I changed my mind and I want my change") and one new variable to store the option ("NextAction");

3. Create a State ("Choose product") and define a new Dictionary variable to store the prices of the individual drinks. 
   In the 'Entry' section of the State:

   -- Use an 'Input Dialog' to display the drink options and let the user choose. The output should be stored in a newly 
      created variable ("ChosenDrink");
      
   -- Use an 'If' activity with the Condition "CurrentAmount>=DrinkPrice(ChosenDrink)":
   
      --  If 'True', use an 'Assign' to calculate the "CurrentAmount" by subtracting the price of the drink. Use an 
          'Input Dialog' to display the remaining amount and allow the user to choose between buying a new drink and requesting 
          the change. Store the outcome in a variable ("NextAction"). If the user selects "Get change", use an 'Assign' activity 
          to assign the value 'True' to a global Boolean variable ("GetChange");
          
      --  If 'False', display a corresponding message in a 'Message Box' and use an 'Assign' to change the value of "GetChange" 
          to 'True'.

4. Create a Final State ("Give change"). In the 'Entry' section, use a 'Message Box' to display the value of the "CurrentAmount"
   variable.

5. Create the following Transitions:

   -- From "Select amount" State to "Choose product" State with the Condition - NextAction.Equals("Yes")
   
   -- From "Choose product" State to "Choose product" State with the Condition - NOT GetChange
   
   -- From "Select amount" State to "Get change" State with the Condition - NextAction.Equals("No, I changed my mind. I want 
      the change")
      
   -- From "Choose product" State to "Get change" State with the Condition - GetChange.
 
