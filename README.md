# a_sysadmin_bash_script

A simple bash script to automatically run common linux admininstrative tasks using bash menus .

The script works by prompting the administrator to select the desired administrative task, the script then does the desired job automatically.


### Running the script
![image](https://github.com/user-attachments/assets/d5b0b296-bb8d-48f8-a9ea-c93b9a70b646) <br> <br>
![image](https://github.com/user-attachments/assets/bdf035c5-f1c2-48a6-91ec-3b09732b0904) <br> <br>

### Bash menus
Bash menus are used to create interactive selection interfaces in Bash scripting. They allow users to choose options from a menu and perform actions based on their selections. Bash menus are typically implemented using the select statement, which displays a prompt and a numbered list of options

### PS3 variable and select statement
The PS3 variable refers to the Prompt String 3 variable. It is a special variable used by the "select" statement to customize the prompt displayed when creating menus with the "select" construct.
The select statement is used to create a simple menu system in Bash, where the user can choose options by entering a number corresponding to the displayed choices. The PS3 variable allows you to define the prompt string displayed before the menu options.
~~~ 
PS3="Select a task"  
select ITEM in "Add User" "List all Processes" "Kill Process" "Install Program" "Quit"
do 
~~~
The snippet above is creating a menu of some admistrative tasks to be selected from then autmoatically run by the script


### REPLY variable
The REPLY variable is automatically set to the user's input. The REPLY variable stores the value entered by the user when prompted for a selection. The user will select an entry from the menu using its corresponding number which is held in the REPLY variable

## Testing the REPLY variable for the task selected

### Add User (REPLY=1)
~~~
if [[ $REPLY -eq 1 ]]
then
    read -p "Enter the username:" username
    output="$( grep -w $username /etc/passwd )"
    if [[ -n "$output" ]]
    then
        echo "$username username already exists."
    else
        sudo useradd -m -s /bin/bash "$username"
        if [[ $? -eq 0 ]]
        then
            echo "The user $username was added successfully" 
            tail -n 1 /etc/passwd
        else
            echo "There was an error adding the user $username."
        fi
    fi
 ~~~
The code snippet above checks if the REPLY variable is 1, reads the username entered by the administrator, confirms the user does not already exists in the /etc/passwd file, then creates the new user.

### Listing Processes (REPLY=2)
~~~
elif [[ $REPLY -eq 2 ]] 
then
    echo "Listing all Processess..."
    sleep 2
    ps -ef
 ~~~
 The code snippet above checks if the REPLY variable is 2, displays "Listing all Processes", pauses for two seconds, then lists the running processes.
 
### Kill Running Process (REPLY=3)
~~~
elif [[ $REPLY -eq 3 ]] 
then
    read -p "Enter Process to Kill:" process
    pkill $process
~~~
The code snippet above checks if the REPLY variable is 3, propmts the administrator to enter the process they want to kill, then kills the process.

### Installing a program (REPLY=4)

~~~
elif [[ $REPLY -eq 4 ]]
then
    read -p "Enter the program to install" program
    sudo apt update && sudo apt install $program
~~~
The code snippet above checks if the REPLY variable is 4, prompts the administrator for the program to install, updates the package list repository on the system, then installs the program

### Quit the script (REPLY=5)

~~~
elif [[ $REPLY -eq 5 ]]
then
    echo "Quitting..."
    sleep 2
    exit
~~~
Quitting the shell session and exiting the script.

~~~
else
    echo "Invalid menu selection" 
fi

done 
~~~
Displays "Invalid menu selection" if the adminstrator selects a number not among the menu list 
