# Testing Kernel with support for large reciprocal modules 


## Summary

The 2 libraries in this package provide support for calculating models with large and sparse reciprocal modules.

This is a very specific scenario where a model has a module with a high number of reciprocal members (usually more than 50 000) but low assignment ratio (which means any given "source" member assigns costs to a small number of "destination" members.

**A typical scenario would be as follows:**

- Module with 200 000 members all interconected with reciprocal assignments
- Every member in this module on average assigns costs to 20 destinations (200 000 members X 20 assignments per member = 4 000 000 assignments)

200 000 members indicates the potential for 40 000 000 000 assignments in the worst case scenario of every member assigning to all other members, but because there are only 4 000 000 assignments, we call this an extremelly sparse reciprocal system (only 0.01% of all possible assignments are in use)

The kernel libraries in this package implement a special algorithm that can solve this particular case of reciprocal system much faster and with a lot lower memory usage than the standard algorithm used by MyABCM Corporate

## How to use this patch

**Here are the steps for using this patch**

1.Stop the MyABCM Corporate Calculation Engine Windows Service

2. Using Windows Explorer, select MyABCM Corporate Calculation Engine installation directory
   Usually C:\Program Files\MyABCM\Corporate\CalcEngine
   
3. Locate and edit the configuration file **Abm.Server.CalcEngine.Service.exe.config**
   
4. Inside the **appSettings** section of the configuration file, add the following line just after the opening tag <appSettings>

   ```xml
   <add key="CalcSparsityThreshold" value="90" />
   ```
   <br>
   This section of the configuration file will look like the image below:
    <img width="668" height="75" alt="image" src="https://github.com/user-attachments/assets/25c05882-a584-4044-b875-98040dc9ad9e" />
   <br>
   
5. Save the changes to your configuration file
     
6. Inside the CalcEngine directory, select the "bin" sub-directory
   
7. Rename the file Abm.Server.CalcEngine.dll to Abm.Server.CalcEngine.dll.original (just to have a copy of the original file)
   
8. Rename the file Abm.Server.CalcEngine.Kernel.dll to Abm.Server.CalcEngine.Kernel.dll.original (just to have a copy of the original file)
   
9. Download and copy the 2 libraries (Abm.Server.CalcEngine.dll and Abm.Server.CalcEngine.Kernel.dll) to the bin directory
    
10. Restart the MyABCM Corporate Calculation Engine Windows Service

## Wrap up / Notes

With the testing kernel active, MyABCM Corporate Calculation Engine will use the new algorithm everytime it detects a reciprocal module with "sparsity" greater than the value set for **CalcSparsityThreshold**.

If you set **CalcSparsityThreshold** to 0 or do not add the parameter, the standard algorithm will be used.

Please not that the algorithm selection is dynamic and will be evaluated for every module inside the model being calculated.

If you want to test the Calculation Engine using the console, you also need to change the **Abm.Server.CalcEngine.Console.exe.config** configuration file
Finally, this preliminary testing kernel is ony used for standard calculation and not for fact generation.

