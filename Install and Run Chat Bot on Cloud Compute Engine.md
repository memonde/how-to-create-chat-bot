### Set up Compute Engine
1. Create a Google Cloud account and log on to it: [Google Cloud Console](https://console.cloud.google.com/)
2. On the left panel, go to "Compute Engine"
3. Select the instance; if this is the first time you use Compute Engine, create a new instance:
	1. Machine type: an e-2 micro machine should be enough for running a chatbot.
	2. Turn one Firewall settings (HTTP and HTTPS traffic)
4. It will take a minute to create the instance; once it is created, you not have the zone and name of your instance.
	1. For example, the name of my instance is `bot`and the zone is `us-central1-a`
5. Click `SSH` next to the instance, it will open the other browser window. 
6. In SSH window, enter below code and hit enter:
```Python
sudo apt-get update
sudo apt-get install -y python3-venv python3-pip git
```
7. Clone your Github Repository
```Python
git clone https://github.com/YOUR_ACCOUNT/YOUR_REPOSITORY.git
```
	When you are prompt to enter your username, enter the Personal Access Token as username, and leave password blank.
1. Enter the directory
```Python
cd YOUR_REPOSITORY
```
9. Install Python 3 virtual environment
```Python
python3 -m venv venv
```
10. Activate this virtual environment
```Python
source venv/bin/activate
```
11. Install all required packages
```Python
pip install -r requirements.txt
```
12. Run below command; this command will ensure VM will continue to run this even with you disconnect from SSH
```Python
nohup python3 YOUR_REPOSITORY_FILE.py > bot.log 2>&1 &
```
13. Check the bot.log; make sure there is no error 
```Python
tail -f bot.log
```
	Ctrl + C to exit the log.

### Handy commands
* Check the process that is running:
```Python
ps aux | grep YOUR_REPOSITORY_FILE.py
```
	In this step, you will see the PID (Process ID) on the second line.
* To stop the current instance running:
```Python
kill <pid>
```
* Check the bot.log: when you notice your bot stop running, you can check the log and see what's the error message
```Python
tail -f bot.log
```
	Ctrl + C to exit the log
* Update local file from Github repository
```Python
cd YOUR_REPOSITORY
git pull
```
	When you are prompt to enter your username, enter the Personal Access Token as username, and leave password blank.
* Restart your bot
```Python
source venv/bin/activate
nohup python3 YOUR_REPOSITORY_FILE.py > bot.log 2>&1 &

```
