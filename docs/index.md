# Modifying an Existing Scrips to Work with Classes
**Dev:** *CMathews*
**Date:** *8.31.21*

## Introduction
The following document outlines the steps taken to modify and update a Python script to work with classes.

## Process
1.	Write the script in PyCharm: I struggled with the data formatting that occurs upon the initial load/read of the file data.  I also had persistent issues with adding a new item to the list that appears to have been caused by the missing return in the if statement.
```
# ------------------------------------------------------------------------ #
# Title: Assignment 08
# Description: Working with classes

# ChangeLog (Who,When,What):
# RRoot,1.1.2030,Created started script
# RRoot,1.1.2030,Added pseudo-code to start assignment 8
# CMathews,8.29.2021,Modified code to complete assignment 8
# ------------------------------------------------------------------------ #

# Data -------------------------------------------------------------------- #
strFileName = 'products.txt'
objFile = None
lstOfProductObjects = []
dicRow = {}
lstTable = []
strChoice = ""
strProduct = ""
fltPrice = ""
strStatus = ""

# --- Make the Class ---
class Product:
    """Stores data about a product:

    properties:
        product_name: (string) with the product's name
        product_price: (float) with the product's standard price
    methods:
        __str__: (string) product_info (product name and price)
    changelog: (When,Who,What)
        RRoot,1.1.2030,Created Class
        CMathews,8.29.2021,Modified code to complete assignment 8
    """

    # -- Constructor --
    def __init__(self, product_name, product_price):
        try:
            self.product_name = product_name
            self.product_price = product_price
        except Exception as e:
            raise Exception("\nError")

    def __str__(self):
        return str(self.product_name) + "," + str(self.__product_price)

    # -- Properties --
    @property
    def product_name(self):
        return str(self.__product_name).title()

    @product_name.setter
    def product_name(self, value):
        if str(value).isnumeric() == False:
            self.__product_name = value
        else:
            raise Exception("\nProduct names cannot be numbers.")

    @property
    def product_price(self):
        return str(self.__product_price)

    @product_price.setter
    def product_price(self, value):
        if str(value).isnumeric() == True:
            self.__product_price = value
        else:
            raise Exception("\nProduct prices must be numbers.")


# Processing  ------------------------------------------------------------- #
class FileProcessor:
    """Processes data to and from a file and a list of product objects:

    methods:
        save_file(file_name, list_of_product_objects):
        read_file(file_name): -> (a list of product objects)

    changelog: (When,Who,What)
        RRoot,1.1.2030,Created Class
        CMathews,8.29.2021,Modified code to complete assignment 8
    """

    @staticmethod
    def save_file(file_name, lstOfProductObjects):
        """Writes dictionary data to text file

        :param: file_name: (object) of text file
        :param: lstOfProductObjects: (list) of dictionary rows
        :return: (list) of dictionary rows, (string) message
        """
        file = open(file_name, "w")
        for row in lstOfProductObjects:
            file.write(row.__str__() + "\n")
        file.close()
        print("\nYour data has been saved!")

    @staticmethod
    def read_file(file_name):
        """Reads data from a file into a list of dictionary rows

        :param: file_name: (string) with name of file:
        :param: lstOfProductObjects: (list) to be filled with file data:
        :return: (list) of dictionary rows
        """
        lstOfProductObjects.clear()
        try:
            file = open(file_name, "r")
            for line in file:
                product, price = line.split(",")
                row = {"Product": product.strip(), "Price": price.strip()}
                lstOfProductObjects.append(row)
            file.close()
        except:
            print("\nThere is no current data.")
        return lstOfProductObjects

# Presentation (Input/Output)  -------------------------------------------- #
class IO:

    @staticmethod
    def print_menu():
        print("""
                Menu:
                1 - Show Current Data
                2 - Add New Data
                3 - Save Data
                4 - Exit
                """)

    @staticmethod
    def input_menu():
        choice = str(input("Which option would you like to perform? ").strip())
        return choice

    @staticmethod
    def show_data():
        """ Shows current data in list

        :param: lstOfProductObjects: (list) of product names and prices
        :return: ()
        """
        if lstOfProductObjects != []:
            print("\nHere is the current data: ")
            for row in lstOfProductObjects:
                row = row.__str__()
                print(row.strip())
        else:
            print("\nThere is no current data.")

    @staticmethod
    def input_yes_no_choice():
        """Gets a yes or no response from the user

        :return: (string)
        """
        choice = str(input())
        choice = choice.strip().lower()
        return choice

    @staticmethod
    def input_press_to_continue(optional_message=''):
        """Pause program and show message before continuing

        :param: optional_message:
        :return:
        """
        print(optional_message)
        input('Press Enter to continue.')

    @staticmethod
    def add_product_to_list():  # Get user input and add it to the list.
        """Adds user data to the dictionary list

        :param: product_name: (string) with the product's name
        :param: product_price: (float) with the product's standard price
        :param: lstOfProductObjects: (list) of dictionary rows
        :return: (list) of dictionary rows
        """

        product_name = str(input("Product Name: ").strip())
        while True:
            try:
                product_price = float(input("Price: "))
                if type(product_price) == float:
                    product_details = product_name, product_price
                    lstOfProductObjects.append(product_details)
                    return product_details
                else:
                    continue
            except ValueError:
                print("Product prices must be numbers.")


# Main Body of Script  ---------------------------------------------------- #
try:
    lstOfProductObjects = FileProcessor.read_file(strFileName)  # Load file data.
    IO.show_data()
    while True:
        IO.print_menu()  # Show menu options.
        choice = IO.input_menu()
        if choice == "1":  # Show current data.
            IO.show_data()
            continue
        elif choice == "2":  # Add data to list.
            IO.add_product_to_list()
            print("\nYour data has been added!")
            IO.input_press_to_continue()
            continue
        elif choice == "3":  # Save data to file.
            FileProcessor.save_file(strFileName, lstOfProductObjects)
            IO.input_press_to_continue()
            continue
        elif choice == "4":  # Exit program.
            print("\nAre you sure you want to exit? Unsaved changes will be lost. (Y/N): ")
            strChoice = IO.input_yes_no_choice()
            if strChoice.lower() == "y":
                print("\nGoodbye!")
                break
            else:
                IO.input_press_to_continue()
            continue
        else:  # All non-menu selections.
            print("\nPlease choose from the available options.")

except Exception as e:  # All non-custom errors.
    print("\nError!")
    print(e, e.__doc__, type(e), sep='\n')
```

3.	Run the script in PyCharm (see pdf attachments).
4.	Run the script in the Command Prompt window.
    ![Image1](https://github.com/cmathews10/IntroToProg-Python-Mod08/blob/main/Assignment01CMD.png "Command Prompt Window")
    ![Image1](https://github.com/cmathews10/IntroToProg-Python-Mod08/blob/main/Assignment02CMD.png "Command Prompt Window")
5.  Confirm user input is being saved to the defined file location.
    ![Image1](https://github.com/cmathews10/IntroToProg-Python-Mod08/blob/main/Assignment03Text.png "Text File")

## Summary
This document outlined the steps taken to modify and update a Python script to work with classes.
