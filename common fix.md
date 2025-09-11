===============================================
fix npm not working.md
Hello Everyone! Today in this video I am going to step by step guide you
✅How To Fix Error PS1 Can Not Be Loaded Because Running Scripts Is Disabled On This System?
or How to fix running scripts is disabled on this system?

Step 1: First, you have to need to open the command prompt and run this command.
set-ExecutionPolicy RemoteSigned -Scope CurrentUser 

When you run this command, you can see that your system has set all policies for the current user as remote. It will take a few seconds to complete this process.

Step 2: Now you have to run the second command on your system. This command is:
Get-ExecutionPolicy

When you run this command, your system shows “RemoteSigned.” If you receive this message, your problem will be solved. Now, you have to go to the next step to view the list of policies updated by the last commands.

Step 3: To view their policy, you need to run this command in your command prompt:
Get-ExecutionPolicy -list

That's all... And there you go! Your problem was Solved!!