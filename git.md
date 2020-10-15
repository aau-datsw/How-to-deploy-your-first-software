
## Create a GitHub repository
> Note: The following happens on your local machine. <br>Use your favourite text editor for the following. I prefer VS Code. |

Make sure you have Git installed on your computer. 

We are going to use Git for version control and we will host our repository on GitHub. The server will later `git pull` from GitHub whenever it should update.

Log onto https://github.com. Under Repositories, add a new private repository called `example-com` (you should of course replace `example-com` with your own domain). Initialize the repository with a README or add it later, it doesn't matter much. 

Once created, copy the URL to your repository and open your terminal. 
Run the commands: 

    cd ~
    git clone https://github.com/your-username/example-com
    cd example-com

This is where we will be writing all the code that goes onto our server. In fact, let’s do the same on the server immediately. SSH into your server and run the following: 

    cd ~
    git clone https://github.com/your-username/example-com
    cd example-com

Close the SSH server session and go back to your local terminal. 