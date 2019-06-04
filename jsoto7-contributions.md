```
// imported libraries

import java.util.Scanner;   // keyboard & file inputs & outputs
import java.io.File;        // connecting to external files
import java.io.IOException; // handle unexpected input/output runtime errors
import java.io.FileWriter;  // writing data to external file

class Main 
{
  // constants

  static final String NAME = "Sven";    // name of program 
  static final int EXIT_CHOICE = 3;     // menu choice to exit program

  // external data file containing list of items sold in vending machine    
  static final String INVENTORY_LIST_FILENAME = "ItemsForSale.txt";

  public static void main(String[] args)
  {
    // local variables

    // List of Items to sell
    InventoryList itemsForSale = new InventoryList();

    // to obtain human's input
    Scanner keyboard = new Scanner(System.in);  
  
    String userMenuChoice = "";   // human inputted menu choice
    String newItem = "";          // new item added to inventory
    int customerSelection = 0;    // customer selection from menu list of items
    ** String PIN = "";              // {[-My Contribution to the Svend Project-]} **

    // read the list of items from  external file
    try
    {
      Scanner externalFile = new Scanner(new File(INVENTORY_LIST_FILENAME));
      
      while (externalFile.hasNext())
      {
        String nextItem = externalFile.nextLine();
        itemsForSale.addItem(nextItem);
      }

    }
    catch (IOException ioe) // if error occurs trying to read data to file
    {
      System.err.println(ioe);
    }

    // setting up the screen (interface / view)

    System.out.println("\n\n");

    System.out.println("Hello, I am the world's first smart vending machine. My name is " + NAME + "!");

    System.out.println("\n\n");
    
    displayMenu();

    userMenuChoice = keyboard.nextLine().trim();  // trim leading & trailing spaces
    userMenuChoice = userMenuChoice.trim();

    if (userMenuChoice.equals("1") || userMenuChoice.equals("A") || userMenuChoice.equals("a"))
    {
      ** System.out.println("Please enter PIN: "); // ----------------------------------------------- **
      ** PIN = keyboard.nextLine();                // {[- My Contribution to the Svend Project -]} **
      ** if (PIN.equals("0000"))                   // ----------------------------------------------- **
      {
        System.out.println("[-]ADMIN MODE[-]");
        System.out.print("What is your new item? ");
        newItem = keyboard.nextLine();
        itemsForSale.addItem(newItem);
        System.out.println("Your New Item Is: ");
        for (int i = 0; i < itemsForSale.numberOfItems(); i++)
      {
        System.out.println((i + 1) + "." + itemsForSale.selectItem(i) + "\n");
      }
      }
      ** else if (!(PIN.equals("0000"))) **
      {
        System.out.println("INCORRECT PASSWORD.");
      }

      try
      { 
        FileWriter externalFile = new FileWriter(INVENTORY_LIST_FILENAME, true);  
        // connect to external file, true specifies append mode
        
        // add the new item to the end of the file with the inventory list
        // TODO (Henry) - allow multiple items to be added all at once

        externalFile.write(newItem + "\n");
        externalFile.flush();                      
        externalFile.close();                                    
      }                                                           
      catch (IOException ioe) // if error occurs trying to write data to file
      {
        System.err.println(ioe);
      }

    }
    else if (userMenuChoice.equals("2") || userMenuChoice.equals("C") || userMenuChoice.equals("c"))
    {
      System.out.println("[-]IN CUSTOMER MODE[-]");

      for (int i = 0; i < itemsForSale.numberOfItems(); i++)
      {
        System.out.println((i + 1) + "." + itemsForSale.selectItem(i) + "\n");
      }

      // display the list of items as a numbered list

      System.out.println("What would you like to purchase? [Please type the number.] ");
      customerSelection = keyboard.nextInt();

      System.out.println("Thank You for purchasing: " + customerSelection);
      System.out.println("Have a Nice Day!");



      try
      { 
        FileWriter externalFile = new FileWriter(INVENTORY_LIST_FILENAME, false);  
        // false specifies write mode (thanks KP)

        for (int i = 0; i < itemsForSale.numberOfItems(); i++)
        {
          externalFile.write(itemsForSale.selectItem(i) + "\n");
        }

        System.out.println("The updated list is: ");
        itemsForSale.displayList();      

        externalFile.flush();                      
        externalFile.close();                                    
      }                                                           
      catch (IOException ioe) // if error occurs trying to write data to file
      {
        System.err.println(ioe);
      }

      // TODO
      // display available item by reading those values from an external data file
      // allow customer to input a choice
      // trust the user to select the correct item from the machine & 
      // deposit exact amount of MinichPay (not ApplePay or AndroidPay) money 
      // in our Room 311 cash till on the honesty system
    }

  } // end of main method

  // display menu
  public static void displayMenu()
  {  
    System.out.println("1. (A)dmin Mode");
    System.out.println("2. (C)ustomer Mode");
    System.out.println("Choice: ");
  }

} // end of Main class
```
