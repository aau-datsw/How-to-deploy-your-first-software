# Working with the terminal
Open a terminal app on your computer. This is where you will be doing all server-side work. 
Just to give an intuition of how this is going to work, try typing `ls` and press enter. You should see a listing of folders and files. 

![](assets/getting_a_server/terminal/ls.png)


From here, we can navigate in and out of folders, open files, and much more. To move into the folder `How-to-deploy-your-first-software`, simply type `cd How-to-deploy-your-first-software`: 

![](assets/getting_a_server/terminal/cd.png)


Typing `ls` again gives you the contents of the `How-to-deploy-your-first-software` folder: 

![](assets/getting_a_server/terminal/cd_ls.png)


To create a new file, type `nano my-file.txt`, and the following will appear: 

![](assets/getting_a_server/terminal/nano.png)


Write some stuff in here. This is practically the same as opening the file in Notepad or any other editor, we are just doing it directly in the terminal with nano.

![](assets/getting_a_server/terminal/nano_edited.png)


To save the file, press `CTRL + X`. This will exit the file, and it will ask whether you want to save the changes or not. Press `Y` and then `Enter`. 

![](assets/getting_a_server/terminal/nano_save_exit.png)


Now, let’s see if the file was indeed created. Type `ls` in the terminal: 

![](assets/getting_a_server/terminal/ls_3.png)


And there is the new file! Open it with `cat` print the content in the terminal: 


![](assets/getting_a_server/terminal/cat.png)


Everything is as it should be. 
This is how we are going to create new/edit existing files on the server, since we only have access through a terminal. 