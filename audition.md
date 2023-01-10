# Utilization Assessment with Python - Audition
## Lab Description: With this challenge we will be working through some of the different ways that the Python psutil module can be used to check resource utilization.

For this lab you will be taking the role as a network security engineer for Globomantics which has been targeted often by the evil Dark Kittens hacking group. One of the duties of this role is ensuring that each of the devices under your control have not been exposed to backdoors or malware from these attacks. To determine this we will use the functionalities detailed above to see if a device or the processes running on it stand out as potentially exploited via excessive resource usage, to see how to do this follow along with the steps. 

### Task 1: Overall Processor Utilization

Step 1: We will begin by opening the virtual environment and accessing the Ubuntu Linux command line via a terminal. 

Step 2: Using your favorite Linux CLI text editor (vim and nano are popular) create a new python text file called overall-cpu-util.py. For our instructions we are going to use the command vim overall-cpu-util.py. This is where we are going to paste in the scripts that we are going to be walking through. 

![](./001.png)

Step 3: Now that you are inside of vim (in this case) you will need to enter insert mode by hitting the 'i' key, when you do this vim should say "INSERT" in the bottom left corner. Now you want to copy and paste in the following code into vim, please check over the pasted contents to ensure they are correct and that all indents have been placed as expected, remember, Python uses the indentation to indicate levels of its logic and if they are not correct the script will not work as expected. 

~~~
import psutil, sys

try:
    while True:
        print("Overall Processor Utilization: {}%".format(psutil.cpu_percent(interval=3, percpu=False)))
except KeyboardInterrupt:
    print("Stopping")
    sys.exit(0)
except Exception as e:
    print(e)
~~~

Step 4: Now we need to save this code by hitting the ESC key then entering in ':wq' key sequence, this will save the contents of the file and exit back to the Linux CLI. 

Step 5: Now its time to run this script and see what its output is. This can be done with the python3 ./overall-cpu-util.py command. 

What you should be seeing now is a running printout of the current overall processor utilization for the virtual environment. As specified in the fifth line of code, the current interval is set to 3 seconds. 

### Task 2: Process Processor Utilization

Now we are going to extend on this functionality and show how we can view the processor utilization for a specific process, to do this you must first stop the script from running by using the Ctrl-C key sequence. 

Step 6: To get started we are going to follow the same process as we did before, and create a new file with vim and paste in the contents of the next script shown below. To do this use the vim ./process-cpu-util.py command.

Step 7: Now that you are inside of vim again, you will need to enter insert mode and copy and paste the following code into vim. Again, please check over the pasted contents to ensure they are correct and that all indents have been placed as expected.

~~~
import psutil, sys

if len(sys.argv) == 2:
    process_id = sys.argv[1]
else:
    print("Enter a single process ID")
    sys.exit()

try:
    p = psutil.Process(int(process_id))
except:
    print("There is no process ID", process_id)
    sys.exit()

print("Showing processor utilization for process ID", process_id)


try:
    while True:
        print("The process {} has a rolling utilization of {}%".format(p.name(),p.cpu_percent(interval=3)))
except KeyboardInterrupt:
    print("Stopping")
    sys.exit(0)
except Exception as e:
    print(e)

~~~

Step 8: Now we need to save this code by hitting the ESC key then entering in ':wq' key sequence.

Step 9: With this script, we have added functionality to specify a process ID at the command line. To generate a process that uses a variable amount of processor use, we will use a separate terminal window to start a ping process and flood packets to the localhost. To initiate this use the sudo ping -f 127.0.0.1 > /dev/null command in that separate window, the command will ask for the sudo password which is <em>ps</em> this will initiate a ping flood locally without any output. 

Step 10: Now you will need to move back to the initial terminal window and run the ps -ef command, this command is used to display the running processes on the device. Within this output look for a process that is described as ping -f 127.0.0.1, then write down the matching process ID from the second column. 

Step 11: Using this process ID, we can now use the saved script by using the python3 ./process-cpu-util.py new-process-id command. If everything works, you should see a running process output for the ping process. The percentage that is shown will be based on the number of process cores that are being shown to the operating system. So for example, if there is only one cpu core then the percentage will be absolute, but if there are two, then the output will be based on a total of 200%, and so on.


